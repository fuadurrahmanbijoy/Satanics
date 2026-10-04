# build-book

**Role.** Turns `bundle.zip` plus the book's details into the finished book PDF: cover, front matter, contents, chapters, appendix, glossary, answers, further reading and index. It works only from the bundle made by `build-content`.

**Start.** Attach this file and `bundle.zip`. Put the book's details in the prompt (section 1). Say: *Start with build-book.* Font zips (names containing "font") may be attached; they are optional.

---

## 0. Run order (agent: follow exactly, add nothing)

1. **Bundle present?** If `bundle.zip` is not attached, reply "Attach bundle.zip." and stop.
2. **Unpack and read status.** `mkdir -p bundle && unzip -oq bundle.zip -d bundle && head -n 3 bundle/manifest.md`. If the status is not `ok`, print the lines starting `fail |` (at most 15), add "Fix the named chapters and rebuild the bundle with build-content." and stop.
3. **Gate** (section 1). Show the details table. Stop and wait. Do nothing else until the reader writes `confirm`.
4. **Extract the builder** (never retype it), from the folder that holds this file:

```
python3 - <<'PY'
import re
s = open('build-book.md', encoding='utf-8').read()
open('book.py', 'w', encoding='utf-8').write(re.search(r'^<<<BOOK>>>\n(.*?)^<<<END>>>\n', s, re.S | re.M).group(1))
PY
```

5. **Write `book.md`** from the confirmed table (one `key: value` line per field, section 1), then run `python3 book.py check book.md`. If it prints problems, show them in one message and return to the gate for those fields only.
6. **Tools.** `python3 -c "import playwright, pyphen" 2>/dev/null || (pip install playwright pyphen --break-system-packages -q && playwright install chromium)`. If they cannot be installed, say so in one line and stop.
7. **Build, one edition at a time, delivering each file the moment it exists.** Run `mkdir -p out`. For `output: colour` or `both`, run `python3 book.py build bundle book.md --edition colour --out out [--fonts <folder>]`, then deliver the PDF it names. For `bw` or `both`, run the same with `--edition bw`, then deliver that PDF. The cover is already inside each PDF (first and last pages). Add `--fonts` only when font zips were attached.
8. **Exactly two PDFs, never more.** Deliver only the file(s) the script names in `out/`: at most one colour and one black-and-white PDF. Never deliver a separate cover, a content proof, a partial build or any other file. If the script stops with an error, report it in one line and deliver nothing.
9. **Final message:** one line per file, as the script prints it, for example `wrote Title by Author (Colour Edition).pdf: 312 pages (330 words per page)`. Nothing else. If the script reports text overflow pages, add them to that line.

## 1. The gate

Read the details from the prompt. Show exactly this, then stop. `*` marks mandatory fields; a mandatory field that is missing is shown as `?` and asked for.

```
**Book details: please confirm**
| Field | Value |
| --- | --- |
| title* | … |
| author* | … |
| cover* | <colour name or #rrggbb> |
| output | colour / bw / both  (default: both) |
| style | classic / plain / modern  (default: classic) |
| subtitle, edition, year, publisher, isbn, dedication, blurb, rights, spelling, preface | only those given; the rest "not set" |
Contents: automatic, chapters and sections. Index: automatic. Glossary, answers, further reading: automatic.
Reply `confirm`, or change any field.
```

- `output` and `style` show their defaults when not given.
- Colour names: burgundy, navy, green, black, brown, teal, blue, red, purple, grey, cream, white, orange, maroon; or any `#rrggbb`.
- `edition` is the printing label ("First edition"); `year` defaults to the current year; `spelling` is `british` (default) or `american` and decides "Colour" or "Color" in the file name; `preface` is optional text, entered as `preface:` followed by the text and ending with a line `:end`.
- Refuse to proceed on anything but `confirm` (or changes that you then show again). Never fill a mandatory field yourself.

`book.md` format: `title: …`, `author: …`, `cover: …`, `output: …`, `style: …`, then the optional fields the same way.

## 2. Book rules (what `book.py` builds; reference, not instructions to you)

| Part | Rule |
| --- | --- |
| Order | Front cover · half-title · title page · copyright page · dedication (if given) · Contents · To the Reader · Preface (if given) · chapters · appendix A1 (if any) · Glossary · Answers to Questions · Further Reading (if any) · Index · back cover |
| Numbering | Front matter in lowercase roman numerals from the half-title; chapters begin at page 1 on a right-hand page; back matter continues in arabic. The covers carry no number. |
| Contents | Always built. Chapter titles in small capitals with their Roman numerals, then the numbered sections, with dot leaders to page numbers. Chapter titles and section titles are what cross-references in the text point to. |
| To the Reader | A fixed template, filled from the details: what the book is, how to read the side notes, bold terms, "Word in use" notes, footnotes, questions, where the answers, glossary and index are. It contains a small sample passage that uses the real layout. No writing tokens are spent on it. |
| Preface | Only if the reader supplied text. |
| Copyright page | Built from the optional details: © year author, rights line (default "All rights reserved."), edition, publisher, ISBN. |
| Glossary | Every term in alphabetical order with its definition and the chapter that introduces it. Duplicates were resolved in the bundle (first appearance wins). |
| Answers | Grouped by chapter, by question number. |
| Further Reading | One alphabetical list of all `:read` items, duplicates removed. |
| Index | Built by code from the glossary terms: each term with the pages where it appears, in ranges. A term that appears nowhere in the chapters is left out. |
| Covers | Full-page colour (the chosen colour; light lettering on dark colours, dark on light). **classic:** double-ruled frame, spaced capitals, ornament. **plain:** centred title between thin rules. **modern:** author at the top, large title low on the page. The back cover carries the blurb if given. |
| Colour edition | Cream pages, near-black text, figures and plates in the palette (red, blue, green, gold, muted) with ink hatching. Nothing may print white; the build refuses to write a PDF if any figure or plate part would. |
| Black and white edition | White pages, pure black text, the palette mapped to distinct greys, images grey, cover colour turned into its grey with white or black lettering. Everything is pure grey; nothing is redrawn. |
| Output | Exactly two PDFs (colour and black and white), each complete with both covers. Nothing else is ever delivered. |
| File names | `[Title] by [Author] (Colour Edition).pdf` and `[Title] by [Author] (Black and White Edition).pdf` (`Color` when `spelling: american`). |
| Limits | Fonts are embedded and the page size is A4, but there is no PDF/X, bleed or crop marks. Confirm the printer's requirements before printing. |

## 3. Token rules

Deliver nothing but the named PDFs. Never read the bundle's chapters, glossary or answers into the chat. Never print or retype `book.py`. Do not summarise, preview or audit. The only text you write is the gate, any problems list, and the final lines.

<<<BOOK>>>
#!/usr/bin/env python3
"""book.py: bundle + confirmed details -> finished book PDF.
  python3 book.py check book.md
  python3 book.py build BUNDLE_DIR book.md --edition colour|bw [--out DIR] [--fonts DIR]"""
import os, re, sys, json, hashlib, datetime, argparse

KEYS = ('title', 'subtitle', 'author', 'cover', 'style', 'output', 'edition', 'year', 'publisher', 'isbn', 'dedication', 'blurb', 'rights', 'spelling', 'preface')
NAMED = {'burgundy': '#6d1a2a', 'navy': '#1b2a49', 'green': '#1f4d36', 'black': '#151515', 'brown': '#4b2e1e', 'teal': '#1f5557', 'blue': '#1f3f7a',
         'red': '#8b1e1e', 'purple': '#4a2a5a', 'grey': '#5b5b5b', 'gray': '#5b5b5b', 'cream': '#efe3c4', 'white': '#fafafa', 'orange': '#b4531a', 'maroon': '#5a1a1a'}
STYLES = ('classic', 'plain', 'modern')

def meta(path):
    m, pre = {}, False
    for ln in open(path, encoding='utf-8').read().splitlines():
        if pre:
            if ln.strip() == ':end': pre = False
            else: m['preface'] = m.get('preface', '') + ln + '\n'
            continue
        r = re.match(r'(\w+):\s*(.*)$', ln)
        if r and r[1] in KEYS:
            if r[1] == 'preface': pre = True
            else: m[r[1]] = r[2].strip()
    return m
def hexof(c):
    c = (c or '').strip().lower()
    if c in NAMED: return NAMED[c]
    return c if re.fullmatch(r'#[0-9a-f]{6}', c) else None
def lum(h): return (0.299 * int(h[1:3], 16) + 0.587 * int(h[3:5], 16) + 0.114 * int(h[5:7], 16)) / 255
def problems(m):
    p = []
    for k in ('title', 'author', 'cover'):
        if not m.get(k): p.append(f'missing {k}')
    if m.get('cover') and not hexof(m['cover']): p.append('cover must be a colour name (' + ', '.join(sorted(NAMED)) + ') or #rrggbb')
    if m.get('style', 'classic') not in STYLES: p.append('style must be classic, plain or modern')
    if m.get('output', 'both') not in ('colour', 'bw', 'both'): p.append('output must be colour, bw or both')
    if m.get('spelling', 'british') not in ('british', 'american'): p.append('spelling must be british or american')
    if m.get('year') and not re.fullmatch(r'\d{4}', m['year']): p.append('year must be four digits')
    return p

COVER_CSS = """
.cvr{position:absolute;inset:0;background:var(--cv);color:var(--cf);font-family:'IM Fell English','Liberation Serif',Georgia,serif;padding:14mm}
.cvr .t{text-transform:uppercase;letter-spacing:.22em;line-height:1.25}
.cvr .a{text-transform:uppercase;letter-spacing:.3em;font-size:13pt}
.cvr .s{font-style:italic;font-size:14pt;line-height:1.3;max-width:120mm;margin-top:8mm}
.cl .b1{border:1.2pt solid var(--cf);padding:2.5mm;height:100%}
.cl .b2{border:.6pt solid var(--cf);height:100%;display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:16mm}
.cl .o{font-size:18pt;letter-spacing:.6em;margin:12mm 0}
.pl{display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center}
.pl .r{width:40mm;border-top:.8pt solid var(--cf);margin:12mm 0}
.md{display:flex;flex-direction:column;justify-content:space-between}
.md .r{border-top:3pt solid var(--cf);width:100%;margin:8mm 0}
"""
BOOK_CSS = COVER_CSS + """
.fr{height:100%;padding:22mm 25mm 22mm 15mm;display:flex;flex-direction:column;align-items:center;text-align:center}
.page.r .fr{padding:22mm 15mm 22mm 25mm}
.ht{margin-top:70mm;font-family:'IM Fell English','Liberation Serif',serif;font-size:22pt;font-variant:all-small-caps;letter-spacing:.2em}
.t1{margin-top:45mm;font-family:'IM Fell English','Liberation Serif',serif;font-size:30pt;line-height:1.15;font-variant:all-small-caps;letter-spacing:.12em}
.t2{font-style:italic;font-size:13pt;line-height:1.35;max-width:110mm}
.t3{margin-top:30mm;font-size:13pt;font-variant:all-small-caps;letter-spacing:.3em}
.t4{margin-top:auto;font-size:9.5pt;font-variant:all-small-caps;letter-spacing:.15em;line-height:14pt}
.cp{margin-top:auto;align-self:flex-start;text-align:left;font-size:9pt;line-height:13pt}
.dd{margin-top:80mm;font-style:italic;font-size:12pt;line-height:1.5;max-width:90mm}
.toc{display:flex;align-items:baseline;line-height:14pt}
.toc .d{flex:1;border-bottom:1pt dotted var(--ink);margin:0 3pt}
.toc.c{font-variant:all-small-caps;letter-spacing:.08em;font-weight:700;margin-top:7pt}
.toc.s{padding-left:8mm;font-size:9.5pt;line-height:12.5pt}
"""

TOREADER = """## To the Reader

> This book teaches {title} one idea at a time, from the very beginning.

This book is written for {audience} who are meeting the subject for the first time. It assumes no earlier knowledge. Every new word is explained when it first appears, and the book moves slowly wherever an idea is new or difficult.

> The narrow column beside the text holds short notes.

Beside most paragraphs you will find a note in the narrow column at the edge of the page. A note in italics gives the main point of the paragraph in one line. Read these notes first to see the shape of a chapter, and again when you review.

@ Key term | A new word, shown in bold where it first appears, with its meaning in one line.

> A bold word is a new term, and its meaning waits beside it.

A word in **bold type** is a new term. It is shown in bold once, where it is first explained, and its meaning is repeated in the narrow column. After that the word appears in ordinary type. Italics are used for emphasis, for foreign words and for the titles of books.^1

^1: A footnote gives a side point that would interrupt the main argument. Footnotes are numbered within each chapter and appear at the foot of the page.

> Some everyday words have a second, special meaning in this subject.

Where an everyday word has a special meaning here, a note marked "Word in use" says so. Where an idea is both new and hard, a note reads "Go slowly here", and the text slows down with it.

## How a Chapter Runs

> Each chapter has the same shape, so you always know where you are.

A chapter opens with a puzzle, then builds its ideas one section at a time. Each section ends with a short recap. The chapter closes with a summary, a problem to think through, the common mistakes people make, a true account from history, and review questions. Questions are numbered, such as 7.1, and their answers are collected at the back of the book.

> Chapters refer to one another by name, never by page.

When a chapter points to another part of the book, it names the chapter and the section, for example Chapter 3, section 3.2. Use the Contents to find it. At the back you will find a glossary of every term, the answers to the questions, a list of further reading, and an index.
"""

def cover_html(m, edition, back=False):
    h = hexof(m['cover']); L = lum(h)
    if edition == 'bw':
        g = round(L * 255); h = '#%02x%02x%02x' % (g, g, g); fg = '#ffffff' if L < .5 else '#000000'
    else: fg = '#d6b25e' if L < .5 else '#1a1612'
    E_ = E.esc; style = m.get('style', 'classic'); title = E_(m['title']).upper()
    fs = 34 if len(m['title']) <= 18 else 28 if len(m['title']) <= 30 else 22
    sub = f'<div class="s">{E_(m["subtitle"])}</div>' if m.get('subtitle') else ''
    var = f'style="--cv:{h};--cf:{fg}"'
    blurb = f'<div class="s" style="font-size:11pt">{E.inline(m["blurb"])}</div>' if m.get('blurb') else ''
    if back:
        if style == 'classic': return f'<div class="cvr cl" {var}><div class="b1"><div class="b2">{blurb}</div></div></div>'
        return f'<div class="cvr pl" {var}>{blurb}</div>'
    au = f'<div class="a">{E_(m["author"])}</div>'
    if style == 'classic': return f'<div class="cvr cl" {var}><div class="b1"><div class="b2"><div class="t" style="font-size:{fs}pt">{title}</div><div class="o">❦</div>{sub}<div style="margin-top:14mm">{au}</div></div></div></div>'
    if style == 'plain': return f'<div class="cvr pl" {var}><div class="t" style="font-size:{fs}pt">{title}</div><div class="r"></div>{sub}<div style="margin-top:14mm">{au}</div></div>'
    return f'<div class="cvr md" {var}>{au}<div><div class="r"></div><div class="t" style="font-size:{fs + 4}pt">{title}</div>{sub}</div></div>'

def blocks(text, tag):
    out = []
    for ln in text.splitlines():
        if ln.startswith(':' + tag + ' '): out.append((ln.split()[1], []))
        elif out and ln.strip(): out[-1][1].append(ln.strip())
    return out
def tok2code(t): return 'a1' if t.upper() == 'A1' else 'ch' + t.zfill(2)
def refs(labels):
    ns = sorted({int(x) for x in labels if x.isdigit()}); out, i = [], 0
    while i < len(ns):
        j = i
        while j + 1 < len(ns) and ns[j + 1] == ns[j] + 1: j += 1
        out.append(str(ns[i]) if i == j else f'{ns[i]}\u2013{ns[j]}'); i = j + 1
    return ', '.join(out[:10]) + (', \u2026' if len(out) > 10 else '')
def sec(key, title, rows, fmt='arabic', fn=None, idx=False, recto=True):
    return dict(kind='flow', key=key, title=E.esc(title), opener='', rows=rows, fn=fn or {}, hdr=True, idx=idx, recto=recto, fmt=fmt)
def raw(key, html, recto=True): return dict(kind='raw', key=key, title='', html=html, fmt='roman', recto=recto)

def build(bundle, mpath, edition, outdir, fonts):
    global E
    sys.path.insert(0, os.path.abspath(bundle)); import engine as E
    m = meta(mpath); bad = problems(m)
    if bad: sys.exit('; '.join(bad))
    F = E.load(bundle); M = E.manifest(F)
    if M['status'] != 'ok': sys.exit('bundle status is not ok: rebuild it with build-content')
    figs = lambda c: {n: E.txt(b) for n, b in F.items() if n.startswith(c + '-') and n.endswith('.svg')}
    chap = [E.chapter_section(c, E.txt(F[c + '-chapter.md']), figs(c))[0] for c in M['codes']]
    # glossary, answers, reading
    gl = []
    for tok, lines in blocks(E.txt(F['glossary.md']), 'glossary'):
        for l in lines:
            t, _, d = l.partition('|'); gl.append((t.strip(), d.strip(), tok2code(tok)))
    gl.sort(key=lambda x: x[0].lower())
    grows = [E.X('<h2>Glossary</h2>', 1)] + [E.Pr(f'<b>{E.inline(t)}.</b> {E.inline(d)} <i>({E.cname(c)})</i>') for t, d, c in gl]
    arows = [E.X('<h2>Answers to Questions</h2>', 1)]
    for tok, lines in blocks(E.txt(F['answers.md']), 'answers'):
        c = tok2code(tok); body = [re.match(r':a (\S+) \| (.*)', l) for l in lines]; body = [b for b in body if b]
        if not body: continue
        arows.append(E.X(f'<h3>{E.cname(c)} \u00b7 {E.esc(M["info"][c][3])}</h3>', 1))
        arows += [E.Pr(f'<b>{E.esc(b[1])}</b> {E.inline(b[2])}') for b in body]
    seen, rd = set(), []
    for c in M['codes']:
        for l in E.txt(F[c + '-chapter.md']).splitlines():
            if l.startswith(':read '):
                it = l[6:].strip()
                if it.lower() not in seen: seen.add(it.lower()); rd.append(it)
    rd.sort(key=str.lower)
    back = [sec('glossary', 'Glossary', grows), sec('answers', 'Answers to Questions', arows)]
    if rd: back.append(sec('reading', 'Further Reading', [E.X('<h2>Further Reading</h2>', 1)] + [E.Pr(E.inline(x)) for x in rd]))
    index_stub = sec('index', 'Index', [E.X('<h2>Index</h2>', 1)])
    terms = sorted({t for t, _, _ in gl}, key=str.lower)
    # pass 1: page numbers, section pages, index references (cached; identical for both editions)
    key = hashlib.sha1(b''.join(n.encode() + b for n, b in sorted(F.items()))).hexdigest() + ('f' if fonts else '')
    os.makedirs('_work', exist_ok=True); os.makedirs(outdir, exist_ok=True)
    cache = os.path.join('_work', '_pass1.json'); r1 = None
    if os.path.isfile(cache):
        try:
            j = json.load(open(cache)); r1 = j['r'] if j['k'] == key else None
        except Exception: r1 = None
    s1 = chap + back + [index_stub]
    if r1 is None:
        r1 = E.render(s1, None, 'colour', m['title'], fonts, BOOK_CSS, terms)
        json.dump(dict(k=key, r=r1), open(cache, 'w'))
    keys1 = [s['key'] for s in s1]; S = {s['key']: s for s in r1['st']}
    # contents
    def line(cls, left, lab): return E.X(f'<div class="toc {cls}"><span>{left}</span><span class="d"></span><span>{lab}</span></div>')
    toc = [E.X('<h2>Contents</h2>', 1)]
    for c in M['codes']:
        t = E.esc(M['info'][c][3]); left = ('Appendix A' if c == 'a1' else E.roman(int(c[2:]))) + '. ' + t
        toc.append(line('c', left, S[c]['start']))
        for h in r1['hd']:
            if keys1[h['si']] == c and re.match(r'^(\d+|A)\.\d+ ', h['t']): toc.append(line('s', E.esc(h['t']), h['lab']))
    for k, name in (('glossary', 'Glossary'), ('answers', 'Answers to Questions'), ('reading', 'Further Reading'), ('index', 'Index')):
        if k in S: toc.append(line('c', name, S[k]['start']))
    # index
    irows, last = [E.X('<h2>Index</h2>', 1)], ''
    for t in terms:
        if t not in r1['ix']: continue
        if t[0].upper() != last: last = t[0].upper(); irows.append(E.X(f'<h3>{E.esc(last)}</h3>', 1))
        irows.append(E.Pr(f'<b>{E.inline(t)}</b>, {refs(r1["ix"][t])}'))
    # front matter
    ti, au, yr = E.esc(m['title']), E.esc(m['author']), m.get('year') or str(datetime.date.today().year)
    eds = E.esc(' \u00b7 '.join(x for x in (m.get('edition'), m.get('publisher')) if x))
    front = [dict(kind='cover', key='cover', html=cover_html(m, edition)),
             raw('half', f'<div class="fr"><div class="ht">{ti}</div></div>'),
             raw('title', '<div class="fr"><div class="t1">' + ti + '</div><div class="orn">\u2767</div>' + (f'<div class="t2">{E.esc(m["subtitle"])}</div>' if m.get('subtitle') else '')
                 + f'<div class="t3">{au}</div>' + (f'<div class="t4">{eds}</div>' if eds else '') + '</div>'),
             raw('copy', '<div class="fr"><div class="cp">' + f'\u00a9 {yr} {au}<br>' + E.esc(m.get('rights') or 'All rights reserved.') + '<br>'
                 + (E.esc(m['edition']) + '<br>' if m.get('edition') else '') + (E.esc(m['publisher']) + '<br>' if m.get('publisher') else '') + (f'ISBN {E.esc(m["isbn"])}' if m.get('isbn') else '') + '</div></div>', recto=False)]
    if m.get('dedication'): front.append(raw('ded', f'<div class="fr"><div class="dd">{E.inline(m["dedication"])}</div></div>'))
    tr = E.parse(TOREADER.replace('{title}', m['title']).replace('{audience}', 'readers'), {}, 'F')
    front += [sec('contents', 'Contents', toc, 'roman'), sec('toreader', 'To the Reader', tr['rows'], 'roman', tr['fn'])]
    if m.get('preface', '').strip():
        ps = [p.strip().replace('\n', ' ') for p in re.split(r'\n\s*\n', m['preface'].strip())]
        front.append(sec('preface', 'Preface', [E.X('<h2>Preface</h2>', 1)] + [E.Pr(E.inline(p), 'first' if i == 0 else '') for i, p in enumerate(ps)], 'roman'))
    final = front + chap + back + [sec('index', 'Index', irows), dict(kind='cover', key='backcover', html=cover_html(m, edition, True))]
    lab = ('Black and White' if edition == 'bw' else ('Color' if m.get('spelling') == 'american' else 'Colour')) + ' Edition'
    name = re.sub(r'[\\/:*?"<>|]', '', f'{m["title"]} by {m["author"]} ({lab}).pdf')
    r = E.render(final, os.path.join(outdir, name), edition, m['title'], fonts, BOOK_CSS)
    tw = sum(int(M['info'][c][4]) for c in M['codes'])
    print(f"wrote {name}: {r['n']} pages ({round(tw / max(1, r['n']))} words per page)" + (f"; text overflows on pages {', '.join(r['ov'])}" if r['ov'] else '') + ('' if r['fonts'] else '; fonts: fallback'))

if __name__ == '__main__':
    ap = argparse.ArgumentParser(); sp = ap.add_subparsers(dest='cmd', required=True)
    c = sp.add_parser('check'); c.add_argument('meta')
    b = sp.add_parser('build'); b.add_argument('bundle'); b.add_argument('meta'); b.add_argument('--edition', choices=('colour', 'bw'), required=True); b.add_argument('--out', default='.'); b.add_argument('--fonts')
    A = ap.parse_args()
    if A.cmd == 'check':
        p = problems(meta(A.meta)); print('\n'.join(p) if p else 'ok')
    else: build(A.bundle, A.meta, A.edition, A.out, A.fonts)
<<<END>>>
