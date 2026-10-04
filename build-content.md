# build-content

**Role.** Merges every chapter package into one validated bundle, `bundle.zip`, which is the only input `build-book` accepts. Optionally also lays the chapters out as a proof PDF, `content.pdf` (chapters only: no cover, front matter, glossary, answers or index).

**Start.** Attach this file and one zip that holds all the `chNN-package.zip` files (the appendix package `a1-package.zip` too, if there is one). Say: *Start with build-content.* Optional in the same prompt: `title: <running title for content.pdf>`.

---

## 0. Run order (agent: follow exactly, add nothing)

1. **Input present?** If no zip (or folder of zips) is attached, reply "Attach the zip of chapter packages." and stop.
2. **Extract the engine** (never retype it), from the folder that holds this file:

```
python3 - <<'PY'
import re
s = open('build-content.md', encoding='utf-8').read()
open('engine.py', 'w', encoding='utf-8').write(re.search(r'^<<<ENGINE>>>\n(.*?)^<<<END>>>\n', s, re.S | re.M).group(1))
PY
```

3. **Collect, validate, merge:** `python3 engine.py collect <the attached zip or folder> --out bundle.zip`. It finds every package at any depth, unpacks them, keeps the newest copy of any file that appears twice, validates, merges, and prints one summary line plus any failures.
4. **Deliver `bundle.zip` at once**, whether or not failures were found (a failed bundle carries `status: failed` in its manifest, and `build-book` refuses it).
5. **Final message.**
   - Status ok: one line, for example `bundle.zip delivered: 15 chapters + appendix, 0 failures.` Then ask: *Also build content.pdf? (yes / no)*. Stop and wait.
   - Status failed: the failure lines as printed (at most 30), then one line: *Fix the named chapters with build-chapter, then rerun.* Do not offer the PDF.
6. **If the reader answers yes:**
   - Make sure the tools exist: `python3 -c "import playwright, pyphen" 2>/dev/null || (pip install playwright pyphen --break-system-packages -q && playwright install chromium)`. If they cannot be installed, say so in one line and stop; the bundle stands.
   - Run `python3 engine.py content bundle.zip --out content.pdf --book "<title or Content Proof>"`. Fonts are optional: if the reader attached font zips (names containing "font"), add `--fonts <folder holding them>`; otherwise omit it.
   - Deliver `content.pdf` at once. Final line: the one line the script printed.
7. If the reader answers no, finish with nothing further.

## 1. Token rules

Never read chapter files into the chat. Never print or retype `engine.py`. Do not summarise, preview or audit. The script prints failures only; do not rerun it unless the input changed. No explanation beyond the lines named above.

## 2. What the bundle holds

`bundle.zip`, flat:

| File | Content |
| --- | --- |
| `manifest.md` | `status: ok or failed`, counts, one `chapter | chNN | roman | title | words | figures | terms | questions` line per chapter, then any `fail | chNN | message` lines |
| `ch01-chapter.md` … `chNN-chapter.md`, `a1-chapter.md` | the chapters, unchanged |
| `ch01-fig01.svg` …, `ch01-plate.svg` … | all figures and opening plates, unchanged |
| `answers.md` | all answers, in chapter order; each block starts `:answers 07` |
| `glossary.md` | all glossary lines, in chapter order; each block starts `:glossary 07`; a term defined twice is kept only where it first appears (lowest chapter number) |
| `engine.py` | the layout engine, so `build-book` needs nothing else |

**Validation** (by code): chapters run `ch01` to `chNN` with no gap; each has chapter, answers and glossary files; every paragraph has a side summary; bold only at first use; footnotes match; every question has an answer and every answer a question; glossary terms match the side-note terms; every figure is used, has a `viewBox`, and uses only palette classes; no course wording; no leftover `:todo`; chapter numerals match the file codes.

## 3. Layout rules (what the engine sets; reference, not instructions to you)

| Part | Rule |
| --- | --- |
| Page | A4, no printed margins beyond the layout below. Outer margin 15 mm, inner (gutter) 25 mm, top 20 mm, bottom 22 mm. |
| Grid | Two columns: side column 42 mm, main column 120 mm, gap 8 mm. The side column is on the outer edge (left on left pages, right on right pages). |
| Type | Libre Caslon Text body at 10.5 pt on 14 pt, justified, hyphenated, first line of each paragraph indented 4.5 mm (none after a heading). Side notes 8.5 pt italic on 10.5 pt. Chapter titles and moods in IM Fell English. Fallback: Liberation Serif or Georgia. Fonts are optional; the layout repaginates on its own with whatever font it gets. |
| Headings | Section headings (`##`): bold capitals, letter-spaced, centred between two rules, numbered `N.k` automatically in order (the opening puzzle, the Worked heading and the closing pack are unnumbered). Subsections (`###`): bold small capitals. A heading is never left alone at the foot of a page. |
| Openers | Every chapter starts on a right-hand page: the chapter's own Victorian-engraving plate in a double-ruled frame, "Chapter VII" in small capitals, title, mood, ornament, description, page number at the foot. The appendix opener has the title only. |
| Running heads | Left page: page number, then the book title. Right page: chapter title, then page number. None on openers. |
| Side notes | Summary notes italic; key-term notes with the term in roman bold; "Go slowly here" flags in bold small capitals. A note sits beside its paragraph. |
| Footnotes | At the foot of the page that cites them, set in the main column, numbered within each chapter. |
| Figures | Inline SVG in Victorian engraving style, centred, at most 95 mm high, with caption "Fig. 7.1. …" numbered automatically per chapter. Colour comes from the palette classes only (engraved shade from the hatch classes); the engine strips any other colour, style or fill, and refuses to write a PDF if any figure or plate part would print white. |
| Tables, boxes | Tables: horizontal rules only, small capitals header, introduced in words by the chapter. Boxes: double-ruled, label in small capitals. |
| Questions | Review, Think-it-Through and Pause-and-check items appear as short blocks with their id (7.1, 7.t1, Pause and check 7.p1). |
| Numbering | Arabic from 1 on the first chapter opener; chapters always begin on a right-hand page (a blank left page is added when needed). |
| Limits | The PDF comes from a browser engine: fonts are embedded, but there is no PDF/X, no bleed and no crop marks. Ask the printer's requirements before sending to print. |

<<<ENGINE>>>
#!/usr/bin/env python3
"""engine.py: parse, validate, merge and lay out. Used by build-content and build-book.
  python3 engine.py collect SRC [SRC ...] [--out bundle.zip]
  python3 engine.py content BUNDLE [--out content.pdf] [--book TITLE] [--fonts DIR]
book.py imports this file from the bundle."""
import os, re, sys, io, json, glob, time, zipfile, argparse, base64, html as H

HERE = os.path.dirname(os.path.abspath(__file__))
EXAM = re.compile(r"\b(past papers?|past questions?|learning (?:objectives?|outcomes?)|course (?:objectives?|outcomes?)|syllabus|exam(?:ination)? (?:papers?|years?|questions?)|marking scheme|marks? (?:allocated|awarded|weighting)|course file)\b", re.I)
PAL = re.compile(r'[sf]-(?:ink|red|blue|green|gold|mute|none)|dash|thin|thick|h-(?:lines|dense|cross|dots)')
CLOSE = ('summary', 'think it through', 'where people go wrong', 'in history', 'sources', 'review questions')
PAT = re.compile(r'(?:(?:ch\d\d|a1)-(?:chapter|answers|glossary)\.md|(?:ch\d\d|a1)-fig\d\d\.svg|ch\d\d-plate\.svg)')
SPECIAL = r"(## |### |> |@|\* \* \*|\^\d+:|:\w+)"
PLACE = '<svg viewBox="0 0 400 250" fill="none" stroke="#1a1612" stroke-width="1.4" stroke-linecap="round"><path d="M200 40v170M170 210h60M180 210l20-14 20 14"/><circle cx="200" cy="38" r="6"/><path d="M70 62 200 50 330 62"/><path d="M70 62 40 150M70 62 100 150M330 62 300 150M330 62 360 150"/><path d="M30 150h80a40 26 0 0 1-80 0zM290 150h80a40 26 0 0 1-80 0z"/><path d="M40 166h60M300 166h60" stroke-width=".6"/><path d="M20 230h360" stroke-width=".6"/><path d="M60 236h280M110 242h180" stroke-width=".4"/></svg>'

def esc(s): return H.escape(s, quote=False)
def txt(b): return b.decode('utf-8', 'replace')
def roman(n):
    s = ''
    for v, c in ((1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'), (100, 'C'), (90, 'XC'), (50, 'L'), (40, 'XL'), (10, 'X'), (9, 'IX'), (5, 'V'), (4, 'IV'), (1, 'I')):
        while n >= v: s += c; n -= v
    return s
def cnum(code): return 'A' if code == 'a1' else str(int(code[2:]))
def cname(code): return 'Appendix A' if code == 'a1' else 'Chapter ' + roman(int(code[2:]))

# ---------- hyphenation (soft hyphens) ----------
_H = None
def _hy():
    global _H
    if _H is None:
        _H = ('none', None)
        try:
            import pyphen
            _H = ('py', pyphen.Pyphen(lang='en_GB', left=3, right=3))
        except Exception:
            try:
                t = open(glob.glob('/usr/share/texlive/texmf-dist/tex/generic/hyphen/hyphen.tex')[0], encoding='latin-1').read()
                i = t.index('\\patterns{'); j = t.index('}', i); P = {}
                for p in re.sub(r'%.*', '', t[i + 10:j]).split():
                    L, V = '', [0]
                    for ch in p:
                        if ch.isdigit(): V[-1] = int(ch)
                        else: L += ch; V.append(0)
                    P[L] = V
                _H = ('tex', P)
            except Exception: pass
    return _H
def hy(w):
    k, d = _hy()
    if k == 'py': return d.inserted(w, '\u00ad')
    if k != 'tex': return w
    x = '.' + w + '.'; v = [0] * (len(x) + 1)
    for i in range(len(x)):
        for j in range(i + 1, min(len(x), i + 9) + 1):
            p = d.get(x[i:j])
            if p:
                for q, a in enumerate(p):
                    if v[i + q] < a: v[i + q] = a
    return ''.join(('\u00ad' if 3 <= c <= len(w) - 3 and v[c + 1] % 2 else '') + w[c] for c in range(len(w)))
def hyph(s):
    return ''.join(g if g.startswith('<') else re.sub(r"(?<![&#\w])[a-z]{7,}(?![;\w])", lambda m: hy(m[0]), g) for g in re.split(r'(<[^>]+>)', s))

# ---------- parsing ----------
def inline(s):
    s = H.escape(s, quote=False)
    s = re.sub(r"\*\*(.+?)\*\*", r"<b>\1</b>", s)
    s = re.sub(r"\*(.+?)\*", r"<i>\1</i>", s)
    return re.sub(r"\^(\d+)", r"<sup>\1</sup>", s)
def svgclean(s):
    m = re.search(r'<svg\b.*</svg>', s, re.S)
    if not m: return ''
    s = re.sub(r'<(style|defs|script|image|pattern|filter|linearGradient|radialGradient|mask|clipPath)\b.*?</\1>', '', m[0], flags=re.S | re.I)
    return re.sub(r'\s(?:fill|stroke|style|opacity|fill-opacity|stroke-opacity|filter|mask|clip-path|stroke-width|stroke-dasharray)="[^"]*"', '', s)
def Pr(text, cls='g', side=''): return dict(t='p', side=side, cls=cls, text=hyph(text))
def X(inner, h=0, side=''): return dict(t='x', h=h, html=f'<div class="row"><div class="side">{side}</div><div class="main">{inner}</div></div>')

def parse(text, figs, num):
    meta, rows, fn, terms, qs, reads, warn = {}, [], {}, [], [], [], []
    st = dict(side=[], buf=[], cls='', pend=[], brk=True, bold=set(), words=0, fig=1, sec=0, body=True, used=[])
    L = text.splitlines(); i = 0
    for ln in L:
        for m in EXAM.finditer(ln): warn.append(f"course wording '{m[0]}'")
    def take():
        sd = ''.join(h for _, h in st['side']); has = any(k == 's' for k, _ in st['side']); st['side'] = []
        return sd, has
    def flush():
        if not st['buf']: return
        t = ' '.join(st['buf']); st['buf'] = []
        sd, has = take()
        if not has: warn.append('paragraph without > summary: ' + t[:40])
        bl = [b.lower() for b in re.findall(r"\*\*(.+?)\*\*", t)]
        for tm in st['pend']:
            if not any(tm[:5].lower() in b for b in bl): warn.append(f"term '{tm}' not in bold in its paragraph")
        for b in bl:
            if b in st['bold']: warn.append(f'bold repeated (first use only): {b}')
            st['bold'].add(b)
        st['pend'] = []; st['words'] += len(t.split())
        rows.append(dict(t='p', side=sd, cls=(st['cls'] + (' first' if st['brk'] else '')).strip(), text=hyph(inline(t))))
        st['cls'] = ''; st['brk'] = False
    def blk(inner, h=0):
        flush(); sd, _ = take(); st['pend'] = []
        rows.append(X(inner, h, sd)); st['brk'] = True
    while i < len(L):
        s = L[i].rstrip(); i += 1
        m = re.match(r":(chapter|mood|desc|plate)\s+(.*)", s)
        if m: meta[m[1]] = m[2].strip(); continue
        if s.startswith(':todo'): flush(); warn.append('todo left: ' + s[5:].strip()[:40]); continue
        if not s.strip(): flush(); continue
        if st['buf'] and not re.match(SPECIAL, s): st['buf'].append(s); continue
        flush()
        if s.startswith('### '): blk(f'<h3>{inline(s[4:])}</h3>', 1)
        elif s.startswith('## '):
            t = re.sub(r'^(?:\d+|A)\.\d+\s+', '', s[3:].strip()); lo = t.lower()
            if lo in CLOSE: st['body'] = False
            if st['body'] and 'worked' not in lo and num != 'F':
                st['sec'] += 1; blk(f'<h2><span class="sn">{num}.{st["sec"]}</span> {inline(t)}</h2>', 1)
            else: blk(f'<h2>{inline(t)}</h2>', 1)
        elif s.startswith('> '): st['side'].append(('s', f'<p>{inline(s[2:])}</p>'))
        elif s.startswith('@! '): st['side'].append(('f', f'<p class="flag">{inline(s[3:])}</p>'))
        elif s.startswith('@ '):
            if '|' not in s: warn.append('bad term note: ' + s[:40]); continue
            tm, d = [x.strip() for x in s[2:].split('|', 1)]
            st['side'].append(('k', f'<p class="kt"><b>{inline(tm)}.</b> {inline(d)}</p>'))
            if not d.lower().startswith('word in use'): st['pend'].append(tm); terms.append((tm, d))
        elif s.strip() == '* * *': blk('<div class="fl">* * *</div>')
        elif re.match(r"\^\d+:", s):
            m = re.match(r"\^(\d+):\s*(.*)", s); fn[int(m[1])] = inline(m[2])
        elif s.startswith(':case '): st['buf'].append(s[6:]); st['cls'] = 'case'
        elif s.startswith(':table '):
            cap = s[7:].strip(); tb = []
            while i < len(L) and L[i].lstrip().startswith('|'): tb.append(L[i].strip()); i += 1
            cells = [[c.strip() for c in r.strip('|').split('|')] for r in tb]
            if len(cells) < 2: warn.append('table needs a header row and at least one row'); continue
            hd = ''.join(f'<th>{inline(c)}</th>' for c in cells[0])
            bd = ''.join('<tr>' + ''.join(f'<td>{inline(c)}</td>' for c in r) + '</tr>' for r in cells[1:])
            blk(f'<div class="cap">{inline(cap)}</div><table><thead><tr>{hd}</tr></thead><tbody>{bd}</tbody></table>')
        elif s.startswith(':fig '):
            cap, _, f = s[5:].partition('|'); f = f.strip(); st['used'].append(f)
            art = svgclean(figs[f]) if f in figs else ''
            if not art: warn.append('figure file not found: ' + f); art = '<div class="cap">[figure to come]</div>'
            blk(f'<div class="fig">{art}</div><div class="figcap"><span class="sc">Fig. {num}.{st["fig"]}.</span> {inline(cap.strip())}</div>'); st['fig'] += 1
        elif s.startswith(':box '):
            lab = s[5:].strip(); body = []
            while i < len(L) and L[i].strip(): body.append(L[i].strip()); i += 1
            blk(f'<div class="box"><div><div class="boxl">{inline(lab)}</div><p>{hyph(inline(" ".join(body)))}</p></div></div>')
        elif re.match(r":(?:q|pause)\s", s):
            m = re.match(r":(q|pause)\s+(\S+)\s*\|\s*(.*)", s)
            if not m: warn.append('bad question line: ' + s[:40]); continue
            qs.append(m[2]); lab = ('Pause and check ' if m[1] == 'pause' else '') + m[2]
            blk(f'<p class="q"><span class="qn">{H.escape(lab)}</span> {hyph(inline(m[3]))}</p>')
        elif s.startswith(':read '): reads.append(s[6:].strip())
        elif s.startswith(':'): warn.append('unknown tag: ' + s[:30])
        else: st['buf'].append(s)
    flush()
    pf = meta.get('plate', '').partition('|')[0].strip()
    if pf: st['used'].append(pf)
    if pf and pf not in figs: warn.append('plate file not found: ' + pf)
    if not pf and num not in ('A', 'F'): warn.append('missing :plate')
    cited = {int(n) for r in rows for n in re.findall(r"<sup>(\d+)</sup>", r.get('text', '') + r.get('html', ''))}
    for n in sorted(cited - set(fn)): warn.append(f'footnote {n} cited but not defined')
    for n in sorted(set(fn) - cited): warn.append(f'footnote {n} defined but never cited')
    for k in (('chapter',) if num == 'A' else ('chapter', 'mood', 'desc')):
        if k not in meta: warn.append('missing :' + k)
    return dict(meta=meta, rows=rows, fn=fn, terms=terms, qs=qs, reads=reads, warn=warn, words=st['words'], used=st['used'])

def opener(code, meta, title, figs):
    if code == 'a1':
        return f'<div class="op"><div class="chap" style="margin-top:60mm">Appendix A</div><h1 class="ctitle">{esc(title)}</h1><div class="orn">❦</div><div class="folio">__PN__</div></div>'
    art, cap = PLACE, '[An engraving will be placed here.]'
    f, _, c = meta.get('plate', '').partition('|')
    if f.strip() in figs: art, cap = svgclean(figs[f.strip()]), c.strip()
    return (f'<div class="op"><div class="frame"><div>{art}<div class="plate">{inline(cap)}</div></div></div><div class="chap">{cname(code)}</div>'
            f'<h1 class="ctitle">{esc(title)}</h1><div class="mood">{inline(meta.get("mood", ""))}</div><div class="orn">❦</div>'
            f'<div class="desc">{inline(meta.get("desc", ""))}</div><div class="folio">__PN__</div></div>')

def chapter_section(code, text, figs):
    R = parse(text, figs, cnum(code))
    c = R['meta'].get('chapter', '').split('|'); title = c[1].strip() if len(c) > 1 else code
    want = 'A1' if code == 'a1' else roman(int(code[2:]))
    if c[0].strip().upper() != want: R['warn'].append(f'chapter numeral "{c[0].strip()}" should be "{want}"')
    R['title'] = title
    return dict(kind='flow', key=code, title=esc(title), opener=opener(code, R['meta'], title, figs), rows=R['rows'], fn=R['fn'], hdr=True, idx=True, recto=True, fmt='arabic'), R

def check_svg(name, s, out):
    if 'viewBox' not in s: out.append(f'{name}: no viewBox')
    if re.search(r'#[0-9a-fA-F]{3,8}\b|\b(?:fill|stroke|style)=|<(?:image|script|style|defs|pattern|use|filter|linearGradient|radialGradient|mask|clipPath)\b|href=|url\(', s): out.append(f'{name}: colours, style, images, scripts or links found')
    for cl in re.findall(r'class="([^"]+)"', s):
        for tk in cl.split():
            if not PAL.fullmatch(tk): out.append(f'{name}: unknown class {tk}')

# ---------- bundle ----------
def load(src):
    F = {}
    if os.path.isdir(src):
        for f in glob.glob(os.path.join(src, '**', '*'), recursive=True):
            if os.path.isfile(f): F[os.path.basename(f)] = open(f, 'rb').read()
    else:
        z = zipfile.ZipFile(src)
        for n in z.namelist():
            if not n.endswith('/'): F[os.path.basename(n)] = z.read(n)
    return F
def manifest(F):
    M = dict(status='failed', codes=[], info={}, fails=[])
    for ln in txt(F.get('manifest.md', b'')).splitlines():
        if ln.startswith('status:'): M['status'] = ln.split(':', 1)[1].strip()
        elif ln.startswith('chapter |'):
            p = [x.strip() for x in ln.split('|')]; M['codes'].append(p[1]); M['info'][p[1]] = p
        elif ln.startswith('fail |'): M['fails'].append(ln[6:].strip())
    return M

def collect(srcs, outzip):
    got, diff = {}, [0]
    def put(name, ts, data):
        name = name.lower()
        if name in got and got[name][1] != data: diff[0] += 1
        if name not in got or ts > got[name][0]: got[name] = (ts, data)
    def zscan(z, d):
        for i in z.infolist():
            if i.is_dir(): continue
            b = os.path.basename(i.filename).lower(); data = z.read(i)
            if b.endswith('.zip') and d < 4:
                try: zscan(zipfile.ZipFile(io.BytesIO(data)), d + 1)
                except zipfile.BadZipFile: pass
            elif PAT.fullmatch(b): put(b, tuple(i.date_time), data)
    for src in srcs:
        for f in ([src] if os.path.isfile(src) else glob.glob(os.path.join(src, '**', '*'), recursive=True)):
            if not os.path.isfile(f): continue
            b = os.path.basename(f).lower()
            if b.endswith('.zip'):
                try: zscan(zipfile.ZipFile(f), 0)
                except zipfile.BadZipFile: pass
            elif PAT.fullmatch(b): put(b, tuple(time.localtime(os.path.getmtime(f))[:6]), open(f, 'rb').read())
    nums = sorted({int(n[2:4]) for n in got if n.startswith('ch')})
    codes = ['ch%02d' % n for n in range(1, (max(nums) if nums else 0) + 1)]
    if any(n.startswith('a1-') for n in got): codes.append('a1')
    fails, gseen, gout, aout, info, dups = [], {}, [], [], [], 0
    if not nums: fails.append('none | no chapter files found')
    for c in codes:
        miss = [k for k in ('chapter', 'answers', 'glossary') if f'{c}-{k}.md' not in got]
        for k in miss: fails.append(f'{c} | missing {c}-{k}.md')
        if miss: continue
        T, A, G = (txt(got[f'{c}-{k}.md'][1]) for k in ('chapter', 'answers', 'glossary'))
        figs = {n: txt(d[1]) for n, d in got.items() if n.startswith(c + '-') and n.endswith('.svg')}
        sec, R = chapter_section(c, T, figs)
        fails += [f'{c} | {w}' for w in R['warn']]
        for n, s in figs.items():
            tmp = []; check_svg(n, s, tmp); fails += [f'{c} | {x}' for x in tmp]
            if n not in R['used']: fails.append(f'{c} | figure not used: {n}')
        for nm, tx, tag in (('answers', A, ':answers'), ('glossary', G, ':glossary')):
            if not tx.lstrip().startswith(tag): fails.append(f'{c} | {nm} file must start with {tag}')
            for m in EXAM.finditer(tx): fails.append(f"{c} | course wording '{m[0]}' in {nm}")
        aid = re.findall(r'^:a (\S+) \|', A, re.M)
        for x in sorted(set(R['qs']) - set(aid)): fails.append(f'{c} | no answer for {x}')
        for x in sorted(set(aid) - set(R['qs'])): fails.append(f'{c} | answer without a question: {x}')
        gl = [l.strip() for l in G.splitlines() if '|' in l and not l.startswith(':')]
        gt = {l.split('|')[0].strip().lower() for l in gl}; tt = {t.lower() for t, _ in R['terms']}
        for x in sorted(tt - gt): fails.append(f'{c} | term not in glossary: {x}')
        for x in sorted(gt - tt): fails.append(f'{c} | glossary term without a side note: {x}')
        tok = 'A1' if c == 'a1' else c[2:]; blk = [f':glossary {tok}']
        for l in gl:
            t = l.split('|')[0].strip().lower()
            if t in gseen: dups += 1; continue
            gseen[t] = c; blk.append(l)
        gout.append('\n'.join(blk))
        aout.append('\n'.join([f':answers {tok}'] + [l.strip() for l in A.splitlines() if l.startswith(':a ')]))
        info.append(f"chapter | {c} | {'A1' if c == 'a1' else roman(int(c[2:]))} | {R['title']} | {R['words']} | {len([n for n in figs if '-fig' in n])} | {len(gl)} | {len(R['qs'])}")
    status = 'failed' if fails else 'ok'
    nch = len([c for c in codes if c != 'a1'])
    man = [':bundle', f'status: {status}', f'failures: {len(fails)}', f'chapters: {nch}', f"appendix: {'a1' if 'a1' in codes else 'none'}",
           f'changed-duplicates-resolved: {diff[0]}', f'glossary-duplicates-removed: {dups}'] + info + ['fail | ' + f for f in fails]
    with zipfile.ZipFile(outzip, 'w', zipfile.ZIP_DEFLATED) as z:
        z.writestr('manifest.md', '\n'.join(man) + '\n')
        z.write(os.path.abspath(__file__), 'engine.py')
        for n in sorted(got):
            if n.endswith('-chapter.md') or n.endswith('.svg'): z.writestr(n, got[n][1])
        z.writestr('answers.md', '\n\n'.join(aout) + '\n'); z.writestr('glossary.md', '\n\n'.join(gout) + '\n')
    tw = sum(int(i.split(' | ')[4]) for i in info)
    print(f"{outzip}: {status}; {nch} chapters{' + appendix' if 'a1' in codes else ''}; {len(fails)} failures; {dups} duplicate glossary terms removed; {tw} words, about {round(tw / WPP + 2 * len(codes) + 50)} book pages at {WPP} words per page")
    for f in fails[:30]: print('  ' + f)
    if len(fails) > 30: print(f'  ... {len(fails) - 30} more (see manifest.md)')
    return status

# ---------- layout ----------
def fontcss(extra):
    dirs = [d for d in [extra, os.path.join(os.getcwd(), 'fonts'), os.path.join(HERE, 'fonts'), '/mnt/user-data/uploads', os.getcwd()] if d and os.path.isdir(d)]
    for d in dirs:
        for z in glob.glob(os.path.join(d, '*.zip')):
            if re.search(r'font|caslon|fell', os.path.basename(z), re.I):
                try: zipfile.ZipFile(z).extractall(os.path.join(os.getcwd(), '_fonts'))
                except Exception: pass
    out, found = [], set()
    for d in dirs + [os.path.join(os.getcwd(), '_fonts')]:
        for f in glob.glob(os.path.join(d, '**', '*.[ot]tf'), recursive=True):
            n = os.path.basename(f).lower()
            fam = 'Libre Caslon Text' if 'caslon' in n else ('IM Fell English SC' if 'fellenglishsc' in n.replace(' ', '') or 'english-sc' in n else ('IM Fell English' if 'fell' in n else None))
            if not fam: continue
            w = '400 700' if '[' in n else ('700' if 'bold' in n else '400'); it = 'italic' if 'italic' in n else 'normal'
            found.add(fam); out.append(f"@font-face{{font-family:'{fam}';src:url('file://{os.path.abspath(f)}');font-style:{it};font-weight:{w}}}")
    return '\n'.join(out), found

CSS = """
:root{--ink:#1a1612;--paper:#f0e2bb;--red:#8a2b2b;--blue:#1f4e79;--green:#2f5d3a;--gold:#9a7418;--mute:#8a8174}
@page{size:A4;margin:0}*{box-sizing:border-box}
body{margin:0;font-family:'Libre Caslon Text','Liberation Serif',Georgia,serif;color:var(--ink);font-size:10.5pt}
.page{width:210mm;height:297mm;background:var(--paper);position:relative;overflow:hidden;page-break-after:always}
.page:last-of-type{page-break-after:auto}
.tp{padding:20mm 25mm 22mm 15mm;display:flex;flex-direction:column;height:100%}.page.r .tp{padding:20mm 15mm 22mm 25mm}
.rh{position:absolute;top:10mm;left:15mm;right:25mm;display:grid;grid-template-columns:20mm 1fr 20mm;font-size:8.5pt;font-variant:all-small-caps;letter-spacing:.14em;border-bottom:.5pt solid var(--ink);padding-bottom:1.5mm}
.page.r .rh{left:25mm;right:15mm}.rh span:nth-child(2){text-align:center}.rh span:nth-child(3){text-align:right}
.body{flex:1;min-height:0;overflow:hidden}
.row{display:grid;grid-template-columns:42mm 120mm;column-gap:8mm}.page.r .row{grid-template-columns:120mm 42mm}
.page.r .row>:first-child{grid-column:2;grid-row:1}.page.r .row>:last-child{grid-column:1;grid-row:1}
.side{font-size:8.5pt;line-height:10.5pt;font-style:italic;padding-top:.6pt}.side p{margin:0 0 4pt}.side .kt{font-style:normal}
.main p{margin:0;text-align:justify;text-indent:4.5mm;line-height:14pt}.main p.first,.main p.cont2{text-indent:0}.main p.cont{text-align-last:justify}
.main p.g{text-indent:0;margin:0 0 3pt}
.main p.q{text-indent:0;text-align:left;margin:0 0 5pt}.qn{font-weight:700;font-variant:all-small-caps;letter-spacing:.06em;margin-right:4pt}
h2{font-size:10.5pt;font-weight:700;text-transform:uppercase;letter-spacing:.14em;text-align:center;border-top:.6pt solid var(--ink);border-bottom:.6pt solid var(--ink);padding:2pt 0;margin:13pt 0 8pt}
.sn{margin-right:5pt}
.body>.row:first-child h2{margin-top:0}
h3{font-size:10.5pt;font-weight:700;font-variant:all-small-caps;letter-spacing:.08em;margin:9pt 0 3pt}
.fl{text-align:center;font-size:12pt;margin:7pt 0;letter-spacing:.5em}
.flag{font-weight:700;font-style:normal;font-variant:all-small-caps;letter-spacing:.06em}
.cap{font-variant:all-small-caps;letter-spacing:.08em;font-size:10pt;margin:9pt 0 3pt}
table{border-collapse:collapse;width:100%;font-size:9.5pt;line-height:12.5pt;margin:0 0 9pt}
th{font-variant:all-small-caps;letter-spacing:.06em;text-align:left;font-weight:700;border-top:.6pt solid var(--ink);border-bottom:.6pt solid var(--ink);padding:2pt 4pt}
td{padding:2pt 4pt;vertical-align:top}tbody tr:last-child td{border-bottom:.6pt solid var(--ink)}
.fig svg,.fig img{width:100%;max-height:95mm;height:auto;display:block;margin:6pt auto}
.fig svg,.frame svg{fill:none;stroke:var(--ink);stroke-width:1.2;stroke-linecap:round;stroke-linejoin:round}
.fig svg :where(text),.frame svg :where(text){fill:var(--ink);stroke:none;font-family:'Libre Caslon Text','Liberation Serif',Georgia,serif;font-size:11px}
.s-ink{stroke:var(--ink)}.s-red{stroke:var(--red)}.s-blue{stroke:var(--blue)}.s-green{stroke:var(--green)}.s-gold{stroke:var(--gold)}.s-mute{stroke:var(--mute)}
.f-ink{fill:var(--ink)}.f-red{fill:var(--red)}.f-blue{fill:var(--blue)}.f-green{fill:var(--green)}.f-gold{fill:var(--gold)}.f-mute{fill:var(--mute)}.f-none{fill:none}
.dash{stroke-dasharray:5 3}.thin{stroke-width:.7}.thick{stroke-width:2.4}
.h-lines{fill:url(#pt-lines)}.h-dense{fill:url(#pt-dense)}.h-cross{fill:url(#pt-cross)}.h-dots{fill:url(#pt-dots)}
.figcap{font-size:9pt;font-style:italic;margin:3pt 0 9pt}.figcap .sc{font-variant:all-small-caps;font-style:normal;letter-spacing:.06em}
.box{border:.6pt solid var(--ink);padding:2px;margin:9pt 0}.box>div{border:.6pt solid var(--ink);padding:5pt 8pt}
.boxl{font-variant:all-small-caps;letter-spacing:.14em;font-weight:700;text-align:center;font-size:9.5pt;margin-bottom:3pt}
.box p{margin:0;text-indent:0;text-align:justify;line-height:14pt;font-size:10pt}
.foot{font-size:8.5pt;line-height:10.5pt;display:none}.foot hr{width:25mm;margin:0 0 3pt;border:0;border-top:.5pt solid var(--ink)}
.foot p{margin:0 0 2pt}.foot .in{display:grid;grid-template-columns:42mm 120mm;column-gap:8mm}
.page.r .foot .in{grid-template-columns:120mm 42mm}.page.r .foot .in>div{grid-column:1;grid-row:1}
sup{font-size:7pt;line-height:0}.probe{position:absolute;left:-9999px;top:0;width:120mm}
.op{padding:22mm 15mm 22mm 25mm;display:flex;flex-direction:column;align-items:center;text-align:center;height:100%}
.frame{width:100%;border:.6pt solid var(--ink);padding:2mm}.frame>div{border:.6pt solid var(--ink);padding:6mm 6mm 3mm}
.frame svg,.frame img{width:100%;height:auto;display:block}
.plate{font-family:'IM Fell English','Liberation Serif',serif;font-style:italic;font-size:8.5pt;margin-top:2mm}
.chap{font-size:10pt;font-variant:all-small-caps;letter-spacing:.35em;margin-top:15mm}
.ctitle{font-family:'IM Fell English','Liberation Serif',serif;font-size:30pt;line-height:1.1;margin:5mm 0 3mm;font-weight:400}
.mood{font-family:'IM Fell English','Liberation Serif',serif;font-style:italic;font-size:13pt;line-height:1.35;max-width:110mm}
.orn{margin:7mm 0 5mm;font-size:14pt;letter-spacing:.6em}.desc{font-style:italic;font-size:10pt;line-height:14pt;max-width:95mm}
.folio{position:absolute;bottom:11mm;left:0;right:0;text-align:center;font-size:9pt}
"""
DEFS = ('<svg width="0" height="0" style="position:absolute"><defs>'
 '<pattern id="pt-lines" width="3" height="3" patternUnits="userSpaceOnUse" patternTransform="rotate(45)"><path d="M0 .5H3" style="stroke:var(--ink);stroke-width:.45;fill:none"/></pattern>'
 '<pattern id="pt-dense" width="2" height="2" patternUnits="userSpaceOnUse" patternTransform="rotate(45)"><path d="M0 .5H2" style="stroke:var(--ink);stroke-width:.45;fill:none"/></pattern>'
 '<pattern id="pt-cross" width="3" height="3" patternUnits="userSpaceOnUse"><path d="M0 0L3 3M3 0L0 3" style="stroke:var(--ink);stroke-width:.4;fill:none"/></pattern>'
 '<pattern id="pt-dots" width="3" height="3" patternUnits="userSpaceOnUse"><circle cx="1.5" cy="1.5" r=".5" style="fill:var(--ink)"/></pattern>'
 '</defs></svg>')
WPP = 340
BW = ":root{--ink:#000;--paper:#fff;--red:#3a3a3a;--blue:#585858;--green:#767676;--gold:#949494;--mute:#b0b0b0}img{filter:grayscale(1)}"

JS = r"""(()=>{
const D=window.DATA,LH=14*96/72;let phys=0,A0=null,FMT='roman',FN={},cur=null,SEC=null,SI=0;const ST=[],OVER=[],HEADS=[],IDX={};
const probe=document.createElement('div');probe.className='probe';probe.innerHTML='<div class="main"><p></p></div>';document.body.appendChild(probe);
const rom=n=>{let s='';for(const [v,c] of [[1000,'m'],[900,'cm'],[500,'d'],[400,'cd'],[100,'c'],[90,'xc'],[50,'l'],[40,'xl'],[10,'x'],[9,'ix'],[5,'v'],[4,'iv'],[1,'i']])while(n>=v){s+=c;n-=v;}return s;};
const labAt=n=>FMT==='roman'?rom(n):String(n-A0);
const add=(cls,inner)=>{const d=document.createElement('div');d.className='page '+cls;if(inner!==undefined)d.innerHTML=inner;document.body.appendChild(d);return d;};
function nextPage(inner){phys++;const p=add(phys%2?'r':'l',inner);p.lab=labAt(phys);p.dataset.si=SI;p.idx=!!SEC.idx;return p;}
function blank(){phys++;add(phys%2?'r':'l');}
function lead(){if(SEC.fmt==='arabic'&&A0===null){if((phys+1)%2===0)blank();A0=phys;}FMT=SEC.fmt;if(SEC.recto&&(phys+1)%2===0)blank();}
const body=p=>p.querySelector('.body');
function lines(t,c){const p=probe.querySelector('p');p.className=c;p.innerHTML=t;return Math.round(p.getBoundingClientRect().height/LH);}
function mk(){const n=phys+1,l=labAt(n);const L=SEC.hdr?(n%2?`<span></span><span>${SEC.title}</span><span>${l}</span>`:`<span>${l}</span><span>${D.book}</span><span></span>`):'';
 const p=nextPage(`${L?`<div class="rh">${L}</div>`:''}<div class="tp"><div class="body"></div><div class="foot"><hr><div class="fns"></div></div></div>`);p.fns=new Set();return p;}
const over=p=>{const b=body(p);return b.scrollHeight>b.clientHeight+1;};
const html=r=>r.t==='x'?r.html:`<div class="row"><div class="side">${r.side}</div><div class="main"><p class="${r.cls}">${r.text}</p></div></div>`;
const fnsOf=r=>[...html(r).matchAll(/<sup>(\d+)<\/sup>/g)].map(m=>+m[1]);
function addFn(p,n){if(p.fns.has(n)||!FN[n])return false;p.fns.add(n);const f=p.querySelector('.foot');f.style.display='block';
 const d=document.createElement('div');d.className='in';d.dataset.n=n;d.innerHTML=`<div></div><div><p><sup>${n}</sup> ${FN[n]}</p></div>`;
 const fs=p.querySelector('.fns'),nx=[...fs.children].find(c=>+c.dataset.n>n);fs.insertBefore(d,nx||null);return true;}
function rmFn(p,n){p.fns.delete(n);p.querySelector(`.in[data-n="${n}"]`).remove();if(!p.fns.size)p.querySelector('.foot').style.display='none';}
function mkEl(r){const t=document.createElement('div');t.innerHTML=html(r);const e=t.firstChild;e.dataset.h=r.h||0;return e;}
function put(p,r,force){const b=body(p),el=mkEl(r);b.appendChild(el);const ad=fnsOf(r).filter(n=>addFn(p,n));
 if(!force&&over(p)&&b.children.length>1){el.remove();ad.forEach(n=>rmFn(p,n));return false;}return true;}
function fits(p,r){const b=body(p),el=mkEl(r);b.appendChild(el);const ad=fnsOf(r).filter(n=>addFn(p,n));const ok=!over(p);el.remove();ad.forEach(n=>rmFn(p,n));return ok;}
function cutp(r,k){const tk=r.text.split(' ');let st=[];
 for(let i=0;i<k;i++)for(const m of tk[i].matchAll(/<(\/?)(b|i)>/g)){if(m[1]){const j=st.lastIndexOf(m[2]);if(j>=0)st.splice(j,1);}else st.push(m[2]);}
 return [{...r,text:tk.slice(0,k).join(' ')+st.slice().reverse().map(t=>`</${t}>`).join(''),cls:r.cls+' cont'},
         {...r,side:'',text:st.map(t=>`<${t}>`).join('')+tk.slice(k).join(' '),cls:'cont2'}];}
function trySplit(p,r){const n=r.text.split(' ').length;if(n<10)return null;let lo=2,hi=n-2,best=0;
 while(lo<=hi){const k=(lo+hi)>>1;if(fits(p,cutp(r,k)[0])){best=k;lo=k+1}else hi=k-1}
 for(let k=best;k>=2&&k>best-60;k--){const [a,b]=cutp(r,k);if(lines(a.text,a.cls)<2)return null;if(lines(b.text,b.cls)>=2)return [a,b];}
 return null;}
D.sections.forEach((sec,si)=>{SEC=sec;SI=si;FN=sec.fn||{};
 if(sec.kind==='cover'){add('cv',sec.html);return;}
 lead();const sl=labAt(phys+1);
 if(sec.kind==='raw'){nextPage(sec.html.replace('__PN__',labAt(phys+1)));ST.push({key:sec.key,title:sec.title,start:sl,end:sl});return;}
 if(sec.opener)nextPage(sec.opener.replace('__PN__',labAt(phys+1)));
 cur=mk();const q=sec.rows.slice();
 while(q.length){const r=q.shift();if(put(cur,r))continue;let rest=r;
  if(r.t==='p'){const sp=trySplit(cur,r);if(sp){put(cur,sp[0],true);rest=sp[1];}}
  const b=body(cur),carry=[];while(b.children.length>1&&b.lastElementChild.dataset.h==='1'){carry.unshift(b.lastElementChild);b.lastElementChild.remove();}
  cur=mk();carry.forEach(e=>body(cur).appendChild(e));put(cur,rest,true);}
 ST.push({key:sec.key,title:sec.title,start:sl,end:labAt(phys)});});
const pages=[...document.querySelectorAll('.page')];
pages.forEach(p=>{if(p.querySelector('.body')&&over(p))OVER.push(p.lab);
 if(p.dataset.si!==undefined)p.querySelectorAll('h2').forEach(h=>HEADS.push({si:+p.dataset.si,t:h.textContent.replace(/\s+/g,' ').trim(),lab:p.lab}));});
if(D.terms&&D.terms.length){const tx=pages.filter(p=>p.idx&&p.querySelector('.body')).map(p=>[p.lab,p.querySelector('.body').textContent.replace(/\u00ad/g,'').toLowerCase()]);
 for(const t of D.terms){const re=new RegExp('\\b'+t.toLowerCase().replace(/[.*+?^${}()|[\]\\]/g,'\\$&'));const L=[];for(const [l,x] of tx)if(re.test(x))L.push(l);if(L.length)IDX[t]=L;}}
const BAD=new Set();document.querySelectorAll('.fig svg *,.frame svg *').forEach(e=>{const cs=getComputedStyle(e);for(const v of [cs.fill,cs.stroke]){const m=v.match(/rgba?\((\d+), (\d+), (\d+)/);if(m&&+m[1]>=245&&+m[2]>=245&&+m[3]>=245){const pg=e.closest('.page');BAD.add(pg?pg.lab:'?');break;}}});
window.BAD=[...BAD];window.STATS=ST;window.OVER=OVER;window.HEADS=HEADS;window.IDX=IDX;window.NPAGES=pages.length;
})()"""

def render(sections, out=None, edition='colour', book='', fonts=None, extra_css='', terms=None):
    try: from playwright.sync_api import sync_playwright
    except Exception: sys.exit('playwright is missing: pip install playwright pyphen --break-system-packages -q && playwright install chromium')
    fc, found = fontcss(fonts)
    css = fc + CSS + (BW if edition == 'bw' else '') + extra_css
    data = json.dumps(dict(sections=sections, book=esc(book), terms=terms or []), ensure_ascii=False).replace('</', '<\\/')
    tmp = os.path.join(os.getcwd(), '_tmp.html')
    open(tmp, 'w', encoding='utf-8').write(f'<!doctype html><html lang="en"><head><meta charset="utf-8"><style>{css}</style></head><body>{DEFS}<script>window.DATA={data}</script></body></html>')
    with sync_playwright() as p:
        b = p.chromium.launch(); pg = b.new_page(); pg.goto('file://' + tmp); pg.evaluate('document.fonts.ready')
        pg.evaluate(JS)
        res = pg.evaluate('({st:window.STATS,hd:window.HEADS,ix:window.IDX,ov:window.OVER,n:window.NPAGES,bad:window.BAD})')
        if res['bad']:
            b.close(); os.remove(tmp); sys.exit('figure or plate parts render white on pages ' + ', '.join(res['bad']) + ': fix those figures; nothing was written')
        if out: pg.pdf(path=out, width='210mm', height='297mm', print_background=True, prefer_css_page_size=True)
        b.close()
    os.remove(tmp); res['fonts'] = sorted(found)
    return res

def content(src, out, book, fonts):
    F = load(src); M = manifest(F)
    if not M['codes']: sys.exit('no chapters in the bundle')
    secs = []
    for c in M['codes']:
        figs = {n: txt(b) for n, b in F.items() if n.startswith(c + '-') and n.endswith('.svg')}
        secs.append(chapter_section(c, txt(F[c + '-chapter.md']), figs)[0])
    r = render(secs, out, 'colour', book or 'Content Proof', fonts)
    tw = sum(int(M['info'][c][4]) for c in M['codes'])
    print(f"wrote {out}: {r['n']} pages ({round(tw / max(1, r['n']))} words per page)" + (f"; text overflows on pages {', '.join(r['ov'])}" if r['ov'] else '') + ('' if r['fonts'] else '; fonts: fallback'))

if __name__ == '__main__':
    ap = argparse.ArgumentParser(); sp = ap.add_subparsers(dest='cmd', required=True)
    a = sp.add_parser('collect'); a.add_argument('src', nargs='+'); a.add_argument('--out', default='bundle.zip')
    b = sp.add_parser('content'); b.add_argument('src'); b.add_argument('--out', default='content.pdf'); b.add_argument('--book', default=''); b.add_argument('--fonts')
    A = ap.parse_args()
    if A.cmd == 'collect': collect(A.src, A.out)
    else: content(A.src, A.out, A.book, A.fonts)
<<<END>>>
