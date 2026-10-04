# build-chapter

**Role.** Writes one chapter package for the course described in this file. Input: a chapter number in the prompt. Output: `chNN-package.zip` and nothing else.

**Start.** Attach this file and say: *Start with build-chapter, chapter 7.* Any form of the number works (7, ch7, ch07, chapter 7, chap-7). The appendix is `A1`.

**Run (agent: follow exactly, add nothing).**

1. Read the code from the prompt. Normalise it to the folder code `ch07` (appendix: `a1`) and the entry key `CH07` (appendix: `A1`). If no number is given, ask for it and stop.
2. Use only three parts of this file: `## GLOBAL`, `## METHOD`, and the single entry `=== CH07 ===`. Ignore every other entry. If this file is already in your context, use those parts from there. If it is only on disk, print just those parts:
   - `awk '/^## GLOBAL/{f=1} /^<<<CHECK>>>$/{f=0} f' build-chapter.md` for GLOBAL and METHOD
   - `awk -v c=CH07 '/^=== /{p=($2==c)} p' build-chapter.md` for the entry
3. Begin your reply with one line: the code and the entry's title. Then write the files into a folder named `ch07/`, in this order, in parts of about 3,000 words (create, then append; never reprint):
   - `ch07-chapter.md`
   - `ch07-answers.md`
   - `ch07-glossary.md`
   - `ch07-plate.svg` (the opening engraving, from the entry's `plate:`; the appendix has none)
   - `ch07-fig01.svg`, `ch07-fig02.svg`, … exactly as many as the entry's `figures:` says
4. Zip the files flat (no folder inside) as `ch07-package.zip` and deliver it at once (in a chat environment, save it to the outputs folder and present it).
5. Then validate. Extract the checker and run it: `awk '/^<<<CHECK>>>$/{f=1;next} /^<<<END>>>$/{f=0} f' build-chapter.md > check.py` then `python3 check.py ch07 build-chapter.md`. It prints `ok` or failures. Fix each failure with a small `str_replace` patch in the same file, rebuild the zip under the same name, and deliver it again. Never rewrite a file to fix a few lines. Do not rerun the checker unless a patch touched more than a few lines.
6. End with one line: `ch07 done.` If any fact needs the reader's check, add one line `Verify:` with the items. Nothing else.

**Token rules.** No preamble, no restating this file, no explanation of choices, no self-review prose, no reading a file back after writing it, no previews. Check only with the script. Output tagged text and SVG only; never HTML, CSS or page layout.

**Facts.** Make one targeted web search on this chapter's historical case (the entry's `history:`), if search is available. Use only settled, well-known facts elsewhere. Cut anything you cannot support, or list it under `Verify:`.

**The entry is the single source.** Use the entry's figures, terms, titles and cross-references exactly as written. You know nothing about other chapters except what the entry and GLOBAL say. Never mention objectives, topics, exams, marks, years of papers, or past papers.

## GLOBAL
course: MGT-101 Introduction to Business
book: Introduction to Business
spelling: British · dates: day, month, year (12 March 2026) · country: mixed, no single country; the running case is set in Bangladesh, examples and history draw on several countries, and the place is named whenever law or standards differ · currency and numbers: taka for the running case, written Tk 158,000 (international digit grouping); a historical case keeps its own currency
running case: Meghna and Sons — illustrative documented case file. Base facts (FIXED): Meghna and Sons, a family-run bookshop in Dhanmondi, Dhaka, opened 4 April 1996 by Nurul Meghna as sole proprietor with Tk 400,000. In 2025: sales Tk 14,400,000; purchases Tk 9,800,000; rent Tk 720,000; wages Tk 2,940,000; other costs Tk 190,000; profit Tk 750,000; 7 staff; 6,200 titles; shop 900 square feet, rent Tk 60,000 per month.
chosen words (one term, one word):
- use business — also called enterprise, firm, concern
- use owner — also called proprietor (only for a sole owner)
- use company — also called joint stock company, corporation (one word: company)
- use customer — also called buyer, client
- use supplier — also called vendor, seller of inputs
size plan: 16 chapters, ~81200 words, ~321 pages (target 320)
chapters:
1. What Business Is
2. Factors of Production
3. Objectives of Business and What Makes It Succeed
4. Choosing Where a Business Stands
5. The Business Environment
6. Social Responsibility of Business
7. Forms of Business Ownership and the Sole Proprietorship
8. The Partnership
9. The Joint Stock Company
10. The Cooperative Society
11. State Enterprise and Comparing the Forms
12. Foreign Trade
13. Export and Import Procedure
14. Multinational Corporations
15. Institutions that Promote Business in Bangladesh
16. Business Combination and Integration

## METHOD

### 1. The reader

- A first-year student with **no prior knowledge** of the subject. Every subject term is new.
- Comfortable with arithmetic and basic algebra; vocabulary and unfamiliar ideas are the barrier.
- English is a second language: fluent in everyday speech. Technical words need a **simple definition in English, not a translation**.
- Intelligent and motivated: a beginner in the subject, not in thinking.

**Rule:** never assume the word, never assume the idea, never assume the reader is slow.

### 2. Voice and tone

One character: serious, clear, never condescending. The register changes with the job.

| Register | Use it when | How it sounds |
| --- | --- | --- |
| **Storyteller** | Opening a new, complex idea | A puzzle or a moment of tension; the reader wants the answer |
| **Plain authority** | Definitions, rules, principles | Short, exact sentences; no ornament |
| **Patient tutor** | Worked examples and procedures | "We" and "you"; each step announced and explained |
| **Measured professor** | Framing, history, judgement | Dignified, slightly formal, suited to a Victorian-style book |

Tone: encouraging without being patronising. Treat confusion as normal and explainable. Be honest about difficulty, sparingly (*"Many students find this step confusing, because…"*) and then remove the confusion. Never imply the reader should already know something. Give any unfamiliar name from history (person, place, company) one line of context: who, when, where. No jokes at the reader's expense; wit only when it helps an idea stick. Clear is not casual.

### 3. Language

- **Sentences.** New or complex material: 12 to 18 words on average, one idea per sentence when defining. Elsewhere: 18 to 22. Vary the length; a short sentence after a long one lands well. Keep subject and verb close. No stacks of nouns. If "it" could mean two things, repeat the noun.
- **Active voice** and everyday words. Choose the plain word over the grand one when both are correct.
- **Never use:** idioms or figures of speech that do not translate; slang; phrasal verbs where one clear verb exists (*carry out* becomes *perform*, *set up* becomes *establish*, used consistently); hedging stacks (*it could perhaps be argued that…*); and words that dismiss difficulty: *simply, just, obviously, clearly, of course, as everyone knows*.
- **One term, one meaning, one word.** Where a concept has several names, mention the equivalents once, in one sentence, then use only the chosen word from GLOBAL. Never vary a term for style.
- Never hurry. If a step matters, give it room.
- Never use `^` except for footnotes. Write powers as ², ³ or in words ("to the power of 4").

### 4. Defining terms

A new term appears in **bold** (plain bold) at first use, and its definition follows at once:

1. The term in bold.
2. *means* or *is*, plus a plain definition of about 20 words or fewer, in words simpler than the term.
3. A tiny example in the next sentence.
4. A side note repeating the definition in one line: `@ Term | definition`.
5. A glossary entry (fuller definition) in the glossary file.

Rules: never define with a technical term the reader has not met (define that one first); never define in a circle; no translations. After first use the term is ordinary type. Bold is only for key terms at first appearance, never for emphasis. Italics are for emphasis, foreign words and titles of works.

**This chapter defines exactly the terms in its entry's `defines:`, no more and no fewer.** Terms in `assumes:` may be used but are never defined and never bold. If a term from a later chapter is needed early, give a one-line plain-word preview, not bold, and point to where it is taught (chapter, section number and title).

**Words with two meanings** (everyday words with a special subject meaning, such as *account, capital, stock, interest, margin, cost, value, current, goods, balance*, and others the entry lists under `words-in-use:`): add a "Word in use" note, at most three per chapter, only where confusion is likely:

```
@ Account | Word in use. In daily life, a record of money in a bank. In accounting, a named record of one kind of item, such as Cash or Sales.
```

### 5. Which approach for which topic

| Kind of topic | Open with | Then |
| --- | --- | --- |
| New, complex idea (never met) | A puzzle or question, in storyteller mode | Build the idea in layers; end with the plain rule |
| Existing framework or phenomenon | A puzzle, then a concrete example | Draw out the general principle; state the rule; note exceptions |
| Technique or procedure | A concrete example | Worked example in tutor mode; formula afterwards |
| Defining theory with a known origin | A short historical hook | Explain the idea; connect it to a modern case |

When in doubt, start with a question the reader can almost answer.

### 6. The layered spine

Every concept follows this order: **idea** (plain words, why it matters, a puzzle where suitable) → **example** (from the running case or history) → **rule** (definition, principle or formula, stated precisely) → **exceptions** (limits, where it fails, common confusions). Weave in, briefly: **by questions** (What is it? Why does it exist? How does it work? Where does it fail?) to check no layer is missing; **by viewpoint** (owner, manager, employee, customer), a sentence or short paragraph; **by stages** (simple first, detail after the simple version is secure; mark advanced material clearly so a beginner can defer it).

### 7. Length

| Tier | Covers | Length |
| --- | --- | --- |
| **Minor** | Vocabulary, small distinctions | 3 to 8 lines (never fewer than three visible lines), plus a side note |
| **Standard** | Most ideas | 150–300 words |
| **Core** | The idea the section exists to teach | 400–700 words, with an example and exceptions |
| **Landmark** | A defining idea of the discipline | 800–1,500 words, with history, worked example, subsections, moderate citation |

Length follows importance, never frequency. It must never feel hurried or padded. A concept that is new to the reader moves up one tier. **Paragraphs:** one idea each, 4 to 10 lines, and each must be summarisable in one line (the side column carries that summary). **Chapter:** the length in the entry's `words:`; the opening puzzle is half to one page.

### 8. Pace: two speeds

**Standard pace.** Introduce new ideas in small groups (two or three per section). End every section with a recap of two or three lines, written as a paragraph that begins *Before moving on.* in italics. Follow the spine.

**Slow pace.** Use it when a concept is both new and abstract, has more than three moving parts, or depends on an earlier idea that may not have stuck (examples: opportunity cost, double-entry, accruals, marginal analysis, elasticity, the time value of money). In slow pace:

1. One idea at a time; finish one before starting the next.
2. An everyday bridge first (a household budget, a market stall, a part-time job), then the subject version.
3. Two forms of the same idea: words, then a simple diagram or a tiny numerical example.
4. The smallest possible numbers in the first example.
5. A recap after each idea, not only at the end of the section.
6. A closing line: *You should now be able to say…* (one sentence the reader can test against).
7. A side flag: `@! Go slowly here.`

Where the entry says `pace: slow`, the whole chapter leans slow. After slow passages place one or two `:pause` questions.

### 9. Ordering: nothing before its time

- Never use a term before it is introduced. If one must appear early, give a one-line preview and point to where it is taught.
- Open with a "You will need" note built from the entry's `need:` (chapter and section names; no page numbers).
- Start from the reader's own experience (spending, saving, buying, selling) and move to the subject: the everyday bridge first, the subject case confirms it.
- Build each idea on what came before. Early figures are small and round; they grow realistic as confidence grows.

### 10. Examples: two tracks

**Track A: the running case.** Use only the facts in the entry's `case:`, exactly as given; never invent or change a figure. Present it as a documented case file, not a story: dates, figures, decisions, records; no dialogue, no feelings, no narrative colour. Every `:case` paragraph begins *Case file, illustrative.* so no reader mistakes it for a real organisation.

**Track B: a real historical case** (the entry's `history:`). Short, factual, dated, attributed. Never invent history. If a date, figure or quote cannot be verified, leave it out. Prefer settled facts; date any current figures. State which country's rules apply when law, tax or standards differ by place.

### 11. Numbers and calculations (calculation and mixed chapters)

Students must be able to do this by hand in an exam. Never skip a step. Every formula or calculation gets a worked example in this order: **Given** (the data), **Find** (what is asked), **Formula** (in words first, then in symbols, saying what each symbol stands for), **Steps** (every line of arithmetic, with units), **Answer** (in a sentence), **Check** (a quick test that it is sensible).

Progression: first example small and clean, so the method is the only thing to learn; second slightly messier and realistic; then a note on **common slips** (wrong sign, forgotten units, mixed-up periods); practice problems close the chapter. Explain where a formula comes from in a line or two when it helps; full derivations only for landmark formulas. The first time a chart appears, say how to read it: what the axes show and what a point means.

### 12. Descriptive chapters

Replace "worked example" with a **worked case**: *Situation, Question, Reasoning, Conclusion, Check.* It applies an idea to a situation; it is not a model exam answer. Replace the change-the-numbers ladder with a change-the-**situation** ladder: a familiar case, a reshaped case, a new setting, a case that combines earlier ideas. Show distinctions in a short comparison table, introduced in words first; the text explains how to read a row. The book never contains model exam answers or advice on writing them; it builds understanding complete enough that a reader can answer.

### 13. Repetition without boredom

Each key idea returns in different forms: in the text; in its side note; in the *Before moving on* recap; in the chapter summary and review questions; and in later chapters as a short recall note in the side column, written from the entry's `assumes:` and `refs:` (*Recall: opportunity cost, Chapter 3, section 3.2.*). Place one or two `:pause` questions after slow passages.

### 14. Referencing and cross-references

- **Light by default:** name the thinker and year in the text (*Fayol, 1916*). **Moderate for defining topics** (entry says `sources: yes`): cite the founder or landmark work by name, with a short note on why it mattered, in the closing *Sources* section.
- Clarifications and side points go in footnotes. Paraphrase and credit; never quote long passages.
- List a few further-reading items as `:read` lines at the end of the chapter (author, title, year); the book collects them into one list.
- **Cross-references** use chapter number, chapter title, section number and section title, in the side column or a footnote, taken from the entry's `refs:`. Never page numbers.

### 15. Chapter shape

1. Opener tags: `:chapter`, `:mood`, `:desc`, and `:plate` (chapters only).
2. The opening puzzle (half to one page), with no heading; it makes the chapter necessary.
3. The sections in the entry (`##` headings), each following the spine and ending with a recap.
4. The worked case or worked example, as its own `##` section whose heading contains the word *Worked*.
5. The closing pack, as `##` headings in this exact order and wording: **Summary** (a few lines); **Think It Through** (a short problem or case, as `:q` items); **Where People Go Wrong** (common misunderstandings, stated and corrected); **In History** (a true account, from the entry's `history:`); **Sources** (only if the entry says `sources: yes`); **Review Questions** (`:q` items, from the entry's `practice:`).
6. `:read` lines, if any.

Opening puzzle through closing pack must stay within the entry's `words:` (plus or minus 10%; count with `wc -w`, never by feel). Use a definition box (`:box`) once or twice per chapter, no more. Use `* * *` between major parts.

**Appendix A1** is a reference, not a lesson: no opener, no closing pack, no questions. Start with `:chapter A1 | Maths You Will Use`, then short `##` topics, each a plain definition and one tiny worked example; its answers and glossary files exist but may hold only the `:answers` / `:glossary` line.

### 16. Markup (tagged text)

One chapter is one `.md` file. A blank line ends a paragraph. A line that ends without a blank line continues the paragraph.

```
:chapter VII | The Partnership         chapter number in Roman numerals, then title (first line)
:mood <one short phrase>               italic line under the title (a question or an image)
:desc <3 to 4 lines of description>    opener page description (one line of text)
## Heading                             section heading
### Heading                            subsection heading
> one-line summary                     side note summarising the NEXT paragraph
@ Term | one-line definition           key-term side note for the next paragraph
@! Go slowly here.                     slow-pace flag in the side column
**term**                               bold, first appearance of a key term only
*word*                                 italic: emphasis, foreign words, titles
^1 in text; ^1: note on its own line   footnote reference and its text (numbered in chapter order)
* * *                                  ornament between major parts
:case Case file, illustrative. …       a case-file paragraph (still needs a > summary)
:table Caption                         then rows: | Head | Head | then | cell | cell |
:fig Caption | ch07-fig01.svg          figure; the book numbers it (Fig. 7.1); never type the number
:box LABEL                             then one paragraph on the next line (for example IN SHORT)
:q 7.1 | question text                  review question (Think It Through: 7.t1, 7.t2…)
:pause 7.p1 | question text            Pause and check question inside a section
:read Author, Title, Year              a further-reading item
:plate ch07-plate.svg | caption         the chapter's opening engraving (required in chapters; no figure number)
:todo <note>                           reminder to yourself; the checker rejects any left in the file
```

**Placement.** Side-note lines (`>`, `@`, `@!`) go directly above the paragraph they belong to, in the order they should appear; a blank line between them and the paragraph is fine. Every paragraph, including every `:case` paragraph, gets a `>` summary. Add a key-term note wherever a term is defined. Introduce every table in words first. Keep every table and figure under about half a page. Tables have horizontal rules only; the book sets them.

**Sample (the voice in eight lines):**

```
## The Idea of Partnership

> Two people, each lacking something, decide to join forces.

Imagine that two friends, one skilled at choosing books and the other at selling them, decide to open a small bookshop together. Neither can afford the stock alone, and neither can run the shop alone. Before a single crate of books is ordered, one question stands between them and their business: who owns what, and who answers for what?

@ Partnership | Two or more people running a business together and sharing its profits.

The law gives the name **partnership** to such an arrangement. A partnership is a business owned by two or more people who have agreed to run it together and to share its profits.^1
```

### 17. Art: the opening plate and the figures (SVG)

**House style: Victorian engraving.** Every picture looks like a plate from a textbook of about 1850 to 1900: fine ink lines on blank ground, shade built from parallel hatching and cross-hatching, stippled dots for texture, steady outlines, period-correct people, clothes, tools and buildings. No gradients, no solid filled blocks except hatched ones, no modern icons, no photographic effects, no lettering inside a plate. Diagrams follow the same hand: neat technical engravings, ruled lines, plain-word labels.

**Opening plate (every chapter, exactly one).** `ch07-plate.svg`, drawn from the entry's `plate:` subject: a concrete scene or still life tied to the chapter's topic, composed for a landscape frame (`viewBox="0 0 400 260"`). Build it in layers: outline, then hatched shade, then a few details. Aim for 4 to 8 KB; the more shade you do with the hatch classes, the lighter the file. Name it in the opener: `:plate ch07-plate.svg | <a short caption phrase, no figure number>`. The appendix has no plate.

**Figures** (as many as the entry's `figures:` says; it may say zero): one idea each, `ch07-fig01.svg`, `ch07-fig02.svg`, …; `viewBox="0 0 400 250"` or similar; under about 4 KB; plain-word labels (no abbreviations at first use). The caption goes in the `:fig` line without a number. The first time a chart appears, say in the text how to read it.

**Strict rules (the checker rejects any breach):**
- One `<svg>` with a `viewBox` per file, made only of shapes, paths, lines, `text` and `g`. No `fill=`, `stroke=`, `style=`, hex codes, `<style>`, `<defs>`, patterns, gradients, filters, images, scripts, links or `url(...)`.
- **Colour comes only from classes.** The book gives every shape a near-black line and no fill; add a class only to depart from that. Stroke: `s-ink`, `s-red`, `s-blue`, `s-green`, `s-gold`, `s-mute`. Fill: `f-ink`, `f-red`, `f-blue`, `f-green`, `f-gold`, `f-mute`, `f-none`. Line: `dash`, `thin`, `thick`. **Engraved shade (ink only): `h-lines`, `h-dense`, `h-cross`, `h-dots`**, which fill a closed shape with hatching. Never use white or any "paper" colour; to leave an area blank, give it no fill.
- Use colour sparingly: a plate is mostly ink with at most one accent colour; a figure uses colour only to separate its parts.
- **Never let colour alone carry meaning.** Every distinction must also be carried by a text label, a dash pattern, hatching or position, so the figure reads in grey (the book also prints a black-and-white edition).

### 18. The answers and glossary files

**`ch07-answers.md`**: first line `:answers 07`. One line per question in the chapter, with the same ids:

```
:a 7.1 | <short answer: one to three sentences; for a calculation, the result and its key step>
```

Answer every `:q` and every `:pause`. Answers are short and agree with the entry's figures. They are answers to this book's own questions, never model exam answers.

**`ch07-glossary.md`**: first line `:glossary 07`. One line per term defined in this chapter (exactly the `@` terms, exactly the entry's `defines:`), in order of appearance:

```
Term | fuller definition in plain words, about 30 to 60 words, with a tiny example if it helps
```

### 19. Silent checks (while writing; report only failures)

1. No objectives, topics, exam years, marks, frequencies or question wording from any course file appear. *(This overrides everything.)*
2. Every date, name, figure and legal point is verified or cut. Case-file figures equal the entry's exactly.
3. Every term is defined before it is used; bold only at first use; no later-chapter term is bold.
4. Every paragraph has a `>` summary; every footnote reference has its text; every question has an answer; every figure and the plate exist.
5. Writing checklist for each section: opens with a question or example before the rule; every concept reaches its exceptions; each paragraph is one idea; each explanation is at least three full lines; every calculation is worked in full in the standard order; a reader with no background could follow each step without guessing; no idioms, phrasal verbs or dismissive words; one word per concept; slow pace and an everyday bridge for new abstract ideas; a recap closes each section.

The script catches most of items 3 and 4 and the structural rules. Items 1, 2 and 5 are yours.

<<<CHECK>>>
import os, re, sys
code, cat = sys.argv[1], sys.argv[2]
chap = code.startswith('ch')
rd = lambda p: open(p, encoding='utf-8').read() if os.path.isfile(p) else None
P = {k: os.path.join(code, '%s-%s.md' % (code, k)) for k in ('chapter', 'answers', 'glossary')}
T, A, G = rd(P['chapter']), rd(P['answers']), rd(P['glossary'])
bad = ['missing ' + P[k] for k, v in zip(P, (T, A, G)) if v is None]
if bad: print('\n'.join(bad)); sys.exit(1)
C = rd(cat) or ''
m = re.search(r'^=== %s ===\n(.*?)(?=^=== |\Z)' % code.upper(), C, re.S | re.M)
E = m.group(1) if m else ''
if not E: bad.append('no catalogue entry for ' + code.upper())
def fld(k):
    r = re.search(r'^%s:[ \t]*(.*)$' % k, E, re.M)
    return r.group(1).strip() if r else ''
nfig = int((re.findall(r'\d+', fld('figures')) or ['0'])[0])
plan = int((re.findall(r'\d[\d,]*', fld('words')) or ['0'])[0].replace(',', ''))
src = fld('sources').lower().startswith('y')
dm = re.search(r'^defines:[ \t]*\n((?:- .*(?:\n|\Z))+)', E, re.M)
D = {x[2:].split('|')[0].strip().lower() for x in (dm.group(1).splitlines() if dm else [])}
EXAM = re.compile(r"\b(past papers?|past questions?|learning (?:objectives?|outcomes?)|course (?:objectives?|outcomes?)|syllabus|exam(?:ination)? (?:papers?|years?|questions?)|marking scheme|marks? (?:allocated|awarded|weighting)|course file)\b", re.I)
AVOID = re.compile(r"\b(simply|just(?!-in-time)|obviously|clearly|of course|as everyone knows)\b", re.I)
for name, txt in (('chapter', T), ('answers', A), ('glossary', G)):
    for n, ln in enumerate(txt.splitlines(), 1):
        for r in EXAM.finditer(ln): bad.append('%s:%d course wording "%s"' % (name, n, r[0]))
        for r in AVOID.finditer(ln): bad.append('%s:%d avoided word "%s"' % (name, n, r[0]))
st = {'buf': [], 'sum': False, 'pend': []}
bolds, refs, defs, heads, figs, qs, terms, meta = set(), set(), set(), [], [], set(), [], set()
wiu = []
def flush():
    if not st['buf']: return
    t = ' '.join(st['buf']); st['buf'] = []
    if not st['sum']: bad.append('no > summary: ' + t[:40])
    bl = [b.lower() for b in re.findall(r'\*\*(.+?)\*\*', t)]
    for tm in st['pend']:
        if not any(tm[:5].lower() in b for b in bl): bad.append('term not bold in its paragraph: ' + tm)
    for b in bl:
        if b in bolds: bad.append('bold repeated (first use only): ' + b)
        bolds.add(b)
    refs.update(re.findall(r'\^(\d+)', t))
    st['sum'] = False; st['pend'] = []
def blk():
    flush(); st['sum'] = False; st['pend'] = []
SP = re.compile(r'(:\w|> |@!? |#{2,3} |\^\d+:|\* \* \*$)')
L = T.splitlines(); i = 0
while i < len(L):
    s = L[i].rstrip(); i += 1
    m = re.match(r':(\w+)', s)
    if m and m[1] in ('chapter', 'mood', 'desc', 'plate'): meta.add(m[1]); continue
    if m and m[1] == 'todo': flush(); bad.append('todo left: ' + s[:40]); continue
    if not s.strip(): flush(); continue
    if st['buf'] and not SP.match(s): st['buf'].append(s); continue
    flush()
    if s.startswith('### '): blk()
    elif s.startswith('## '): blk(); heads.append(s[3:].strip().lower())
    elif s.startswith('> '): st['sum'] = True
    elif s.startswith('@! '): pass
    elif s.startswith('@ '):
        if '|' not in s: bad.append('bad term note: ' + s[:40])
        else:
            tm, d = [x.strip() for x in s[2:].split('|', 1)]
            if d.lower().startswith('word in use'): wiu.append(tm)
            else: st['pend'].append(tm); terms.append((tm, d))
    elif s.strip() == '* * *': blk()
    elif re.match(r'\^\d+:', s): defs.add(re.match(r'\^(\d+):', s)[1])
    elif s.startswith(':case '): st['buf'].append(s[6:])
    elif s.startswith(':table '):
        while i < len(L) and L[i].lstrip().startswith('|'): i += 1
        blk()
    elif s.startswith(':fig '):
        figs.append(s[5:].partition('|')[2].strip()); blk()
    elif s.startswith(':box '):
        while i < len(L) and L[i].strip(): i += 1
        blk()
    elif re.match(r':(?:q|pause) ', s):
        q = re.match(r':(?:q|pause) (\S+) \|', s)
        if q: qs.add(q[1])
        else: bad.append('bad question line: ' + s[:40])
        blk()
    elif s.startswith(':read '): pass
    elif s.startswith(':'): bad.append('unknown tag: ' + s[:30])
    else: st['buf'].append(s)
flush()
for n in sorted(refs - defs): bad.append('footnote %s cited but not defined' % n)
for n in sorted(defs - refs): bad.append('footnote %s defined but never cited' % n)
if chap:
    for k in ('chapter', 'mood', 'desc'):
        if k not in meta: bad.append('missing :' + k)
    need = ['summary', 'think it through', 'where people go wrong', 'in history'] + (['sources'] if src else []) + ['review questions']
    pos = [heads.index(h) if h in heads else -1 for h in need]
    if -1 in pos or pos != sorted(pos): bad.append('closing pack headings missing or out of order: ' + ', '.join(need))
    else:
        nb = pos[0]
        if not 3 <= nb <= 7: bad.append('%d body sections before Summary (need 3 to 7, worked case included)' % nb)
        if not any('worked' in h for h in heads[:nb]): bad.append('no section heading containing "Worked"')
    if not src and 'sources' in heads: bad.append('Sources heading but entry says sources: no')
    if not qs: bad.append('no :q or :pause questions')
elif 'chapter' not in meta: bad.append('missing :chapter')
if not A.lstrip().startswith(':answers'): bad.append('answers file must start with :answers')
if not G.lstrip().startswith(':glossary'): bad.append('glossary file must start with :glossary')
aid = set(re.findall(r'^:a (\S+) \|', A, re.M))
for x in sorted(qs - aid): bad.append('no answer for ' + x)
for x in sorted(aid - qs): bad.append('answer without a question: ' + x)
tt = {t.lower() for t, _ in terms}
gt = {l.split('|')[0].strip().lower() for l in G.splitlines() if '|' in l and not l.startswith(':')}
for x in sorted(tt - gt): bad.append('term not in glossary: ' + x)
for x in sorted(gt - tt): bad.append('glossary term without a side note: ' + x)
if D:
    for x in sorted(tt - D): bad.append('term not assigned to this chapter: ' + x)
    for x in sorted(D - tt): bad.append('assigned term never defined: ' + x)
if len(wiu) > 3: bad.append('more than three Word in use notes')
for f in figs:
    if not os.path.isfile(os.path.join(code, f)): bad.append('figure file missing: ' + f)
    if not re.fullmatch(r'%s-fig\d\d\.svg' % code, f): bad.append('bad figure name: ' + f)
allf = sorted(x for x in os.listdir(code) if x.endswith('.svg'))
fs = [x for x in allf if not x.endswith('-plate.svg')]
if chap:
    if 'plate' not in meta: bad.append('missing :plate')
    if '%s-plate.svg' % code not in allf: bad.append('plate file missing: %s-plate.svg' % code)
if len(figs) != nfig or len(fs) != nfig: bad.append('figures: entry says %d, chapter uses %d, folder has %d' % (nfig, len(figs), len(fs)))
for f in allf:
    s = open(os.path.join(code, f), encoding='utf-8').read()
    if 'viewBox' not in s: bad.append(f + ': no viewBox')
    if re.search(r'#[0-9a-fA-F]{3,8}\b|\b(?:fill|stroke|style)=|<(?:image|script|style|defs|pattern|use|filter|linearGradient|radialGradient|mask|clipPath)\b|href=|url\(', s): bad.append(f + ': palette classes only (no colours, style, images, scripts, links)')
    for cl in re.findall(r'class="([^"]+)"', s):
        for tk in cl.split():
            if not re.fullmatch(r'[sf]-(?:ink|red|blue|green|gold|mute|none)|dash|thin|thick|h-(?:lines|dense|cross|dots)', tk): bad.append('%s: unknown class %s' % (f, tk))
w = len(T.split())
if plan and not 0.9 * plan <= w <= 1.1 * plan: bad.append('length: %d words against a plan of %d' % (w, plan))
print('\n'.join(bad) if bad else 'ok')
sys.exit(1 if bad else 0)
<<<END>>>

=== CH01 ===
title: What Business Is
purpose: Gives the reader a plain, working picture of what a business is, what it does, and its main branches.
pace: slow — first chapter; teaches how to read the book and the subject's words
style: descriptive
words: 5125
figures: 2
- fig01: business as a cycle: resources in, goods and services out, money back
- fig02: the branches of business drawn as a tree
plate: a draper's shop interior with a long counter, bolts of cloth and a customer, 1880s
sections:
1.1 | Why People Do Business | new idea | 550
1.2 | Trade, Commerce and Industry | framework | 900
1.3 | Branches of Business | framework | 775
1.4 | Scope and Characteristics of Business | framework | 300
concepts:
- 1.1 | exchange and need | standard | needs: none
- 1.1 | business | standard | needs: exchange and need
- 1.1 | profit and loss | minor | needs: business
- 1.2 | trade | standard | needs: profit and loss
- 1.2 | commerce | standard | needs: trade
- 1.2 | industry | standard | needs: commerce
- 1.2 | aids to trade | standard | needs: industry
- 1.3 | extractive industry | standard | needs: aids to trade
- 1.3 | manufacturing industry | standard | needs: extractive industry
- 1.3 | construction | minor | needs: manufacturing industry
- 1.3 | service | standard | needs: construction
- 1.4 | scope of business | minor | needs: service
- 1.4 | characteristics of business | minor | needs: scope of business
- 1.4 | qualities of a good businessperson | minor | needs: characteristics of business
defines:
- Business | An activity that provides goods or services to others in exchange for money, aiming to earn a profit.
- Profit | What remains of a business's income after all its costs have been paid.
- Trade | The buying and selling of goods, carried out to earn a gain from the difference in price.
- Commerce | Trade together with the services that help goods move from producer to customer, such as transport and banking.
- Industry | The branch of business that produces or changes goods, from raw material to finished product.
- Extractive industry | Business that takes natural materials out of the earth, sea or land, such as farming or fishing.
- Manufacturing industry | Business that turns materials into finished goods by making or assembling them.
- Service | Work done for a customer that gives value but is not a physical thing owned afterwards, such as banking or teaching.
assumes: none
words-in-use: profit; service; goods
case: The shop's 2025 figures (see base facts). Activity: buys new books and stationery from publishers, sells to schoolchildren, families and libraries. Classified as trade (buying and selling); supporting services used: bank, courier, insurance of stock at Tk 36,000 a year. No manufacturing. Gross margin 2025: sales 14,400,000 less purchases 9,800,000 = Tk 4,600,000.
history: Josiah Wedgwood, 1759: began his own pottery works in Staffordshire and later built the Etruria works (1769), showing manufacturing industry as a business | search: Wedgwood Burslem 1759 Etruria 1769
refs: none
need: nothing
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: classify five named activities as trade, commerce or industry; explain the difference between a service and a good with new examples; list the aids to trade used by a given market stall; think: a tailor wants to grow into selling ready-made clothing, describe what branches of business are involved; pause: after the definition of business, after the three-way split
sources: no

=== CH02 ===
title: Factors of Production
purpose: Shows the four resources every business combines and what each earns.
pace: slow — first chapter-length look at the resources every business needs; teaches the book's habit of defining before using
style: descriptive
words: 5025
figures: 1
- fig01: four factors of production and the reward each earns
plate: a Victorian brickworks with workmen, a kiln, a clay pit and a foreman with a ledger
sections:
2.1 | Land and Natural Resources | new idea | 550
2.2 | Labour | new idea | 550
2.3 | Capital and Enterprise | new idea | 650
2.4 | How the Factors Combine | framework | 675
concepts:
- 2.1 | factor of production | standard | needs: none
- 2.1 | land | standard | needs: factor of production
- 2.1 | rent as reward | minor | needs: land
- 2.2 | labour | standard | needs: rent as reward
- 2.2 | wages as reward | minor | needs: labour
- 2.2 | division of labour | standard | needs: wages as reward
- 2.3 | capital | standard | needs: division of labour
- 2.3 | interest as reward | minor | needs: capital
- 2.3 | enterprise | standard | needs: interest as reward
- 2.3 | profit as reward | minor | needs: enterprise
- 2.4 | combining the factors | standard | needs: profit as reward
- 2.4 | mobility of factors | standard | needs: combining the factors
- 2.4 | importance of each factor | standard | needs: mobility of factors
defines:
- Factor of production | A resource used to make goods or provide services; economists name four: land, labour, capital and enterprise.
- Land | All natural resources a business uses, including ground, water, minerals and forests.
- Labour | The physical and mental work people give to a business, paid for with wages.
- Capital | Man-made goods and money used to produce other goods, such as machines, tools and stock.
- Enterprise | The act of organising the other factors and accepting the risk of loss in the hope of profit.
- Division of labour | Splitting a job into small tasks so that each worker repeats one task and becomes faster.
assumes: Business, Profit, Trade
words-in-use: capital; land
case: Factors at the shop in 2025: land = the rented 900 square feet at Tk 60,000 a month (Tk 720,000 a year); labour = 7 staff, total wages Tk 2,940,000; capital = stock Tk 3,500,000 plus fittings Tk 600,000, total Tk 4,100,000; enterprise = Nurul Meghna, reward Tk 750,000 profit.
history: Henry Ford, 1913: moving assembly line at Highland Park, Michigan, combining labour, capital and division of labour; Model T assembly time fell sharply | search: Highland Park assembly line 1913 Model T
refs: Chapter 1, 'What Business Is', section 1.1 'Why People Do Business' — business and trade come before the factors; Chapter 1, 'What Business Is', section 1.3 'Branches of Business' — branches of business that use the factors
need: you-will-need: Chapter 1 'What Business Is', section 1.1 'Why People Do Business'; Chapter 1 'What Business Is', section 1.3 'Branches of Business'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: name factor and reward for given incomes; explain why enterprise differs from labour; compare two shops' factor mix; think: an owner chooses between a machine and three workers; pause: after capital definition, after the reward table
sources: no

=== CH03 ===
title: Objectives of Business and What Makes It Succeed
purpose: Explains why a business exists, what goals it sets, and what a business needs in order to last.
pace: slow — new idea, reader must see profit alongside other goals
style: descriptive
words: 4850
figures: 1
- fig01: economic, social and human goals shown as three overlapping rings
plate: a Victorian model factory village with workers' cottages, a green and a works chimney
sections:
3.1 | The Purpose of a Business | new idea | 675
3.2 | Social and Human Objectives | framework | 675
3.3 | What a Business Needs to Succeed | framework | 900
concepts:
- 3.1 | economic objective | standard | needs: none
- 3.1 | profit as survival | standard | needs: economic objective
- 3.1 | growth | standard | needs: profit as survival
- 3.2 | social objective | standard | needs: growth
- 3.2 | human objective | standard | needs: social objective
- 3.2 | reconciling economic and social aims | standard | needs: human objective
- 3.3 | prerequisites of success | standard | needs: reconciling economic and social aims
- 3.3 | planning and foresight | standard | needs: prerequisites of success
- 3.3 | customer goodwill | standard | needs: planning and foresight
- 3.3 | sound finance | standard | needs: customer goodwill
defines:
- Objective | A clear aim that a business tries to reach within a stated time.
- Goodwill | The good name a business earns with customers, which brings them back and is worth money.
- Survival | A business's ability to keep operating by covering its costs year after year.
- Economic objective | An aim that can be measured in money, such as profit, sales or market share.
assumes: Business, Profit, Trade, Factor of production, Land, Labour
words-in-use: objective; interest
case: Aims written down for 2026: raise profit from Tk 750,000 to Tk 900,000; supply 12 schools' libraries (4 schools in 2025); train 7 staff in customer service (14 hours each); give 200 books a year to a school library. Return on capital 2025 = 750,000 / 4,100,000 = 18.3%.
history: Cadbury, 1879: moved to Bournville, Birmingham, with a model village built from 1895; combined profit with workers' welfare | search: Cadbury Bournville 1879 village trust 1900
refs: Chapter 1, 'What Business Is', section 1.1 'Why People Do Business' — profit defined; Chapter 2, 'Factors of Production', section 2.3 'Capital and Enterprise' — capital and enterprise
need: you-will-need: Chapter 1 'What Business Is', section 1.1 'Why People Do Business'; Chapter 2 'Factors of Production', section 2.3 'Capital and Enterprise'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: separate economic from social aims in a described firm; give three prerequisites for a florist; think: when does a social aim conflict with profit; pause: after goodwill
sources: no

=== CH04 ===
title: Choosing Where a Business Stands
purpose: Teaches how a business weighs the factors that decide the site of its plant or shop.
pace: standard — framework applied to a single decision
style: descriptive
words: 5125
figures: 1
- fig01: map-style sketch showing market, raw material and transport pulling a site
plate: a Victorian canal wharf with warehouses, barges and a crane, 1860s
sections:
4.1 | Why Location Matters | new idea | 675
4.2 | Pull of Materials and Markets | framework | 675
4.3 | Labour, Power, Land and Services | framework | 775
4.4 | Government and Community Factors | framework | 400
concepts:
- 4.1 | plant location | standard | needs: none
- 4.1 | fixed commitment of a site | standard | needs: plant location
- 4.1 | cost and revenue effects | standard | needs: fixed commitment of a site
- 4.2 | nearness to raw material | standard | needs: cost and revenue effects
- 4.2 | nearness to market | standard | needs: nearness to raw material
- 4.2 | transport and communication | standard | needs: nearness to market
- 4.3 | labour supply | standard | needs: transport and communication
- 4.3 | power and water | standard | needs: labour supply
- 4.3 | land cost and laws | standard | needs: power and water
- 4.3 | banking and insurance services | minor | needs: land cost and laws
- 4.4 | government policy and incentives | minor | needs: banking and insurance services
- 4.4 | climate and environment | minor | needs: government policy and incentives
- 4.4 | safety and security | minor | needs: climate and environment
- 4.4 | weighing the factors | minor | needs: safety and security
defines:
- Plant location | The place where a business builds its factory or opens its shop, chosen by weighing several factors.
- Footfall | The number of people who walk past or enter a shop in a given time.
- Industrial estate | An area planned and serviced by an authority where many factories are grouped together.
assumes: Factor of production, Land, Labour, Objective, Goodwill, Survival
words-in-use: cost; plant
case: Site choice for a second shop, 12 May 2026: Dhanmondi (existing) rent Tk 60,000, 900 sq ft, footfall 410 a day; Mirpur rent Tk 38,000, 700 sq ft, 520 a day, three schools within 600 metres; Uttara rent Tk 52,000, 800 sq ft, 380 a day, parking available. Counts are five-day averages.
history: Tata Steel (Tata Iron and Steel Company), 1907: site at Sakchi (later Jamshedpur) chosen for ore, coal, water and rail links | search: Jamshedpur site selection 1907 Sakchi
refs: Chapter 2, 'Factors of Production', section 2.1 'Land and Natural Resources' — land and natural resources; Chapter 3, 'Objectives of Business and What Makes It Succeed', section 3.3 'What a Business Needs to Succeed' — what a business needs to succeed
need: you-will-need: Chapter 2 'Factors of Production', section 2.1 'Land and Natural Resources'; Chapter 3 'Objectives of Business and What Makes It Succeed', section 3.3 'What a Business Needs to Succeed'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: rank factors for a bakery, a steel mill, a software office; think: a firm may move; pause: after nearness to market
sources: no

=== CH05 ===
title: The Business Environment
purpose: Shows the forces inside and outside a business and how it deals with them.
pace: standard — framework with many parts, all new
style: descriptive
words: 5050
figures: 1
- fig01: a business at the centre with internal and external forces around it
plate: a Victorian harbour-master's office window looking onto ships, cranes and a market square
sections:
5.1 | Inside the Business | new idea | 675
5.2 | Outside the Business | framework | 1125
5.3 | Interaction with the Environment | framework | 650
concepts:
- 5.1 | internal environment | standard | needs: none
- 5.1 | owners, managers and employees | standard | needs: internal environment
- 5.1 | resources and culture | standard | needs: owners, managers and employees
- 5.2 | external environment | standard | needs: resources and culture
- 5.2 | economic forces | standard | needs: external environment
- 5.2 | social and cultural forces | standard | needs: economic forces
- 5.2 | political and legal forces | standard | needs: social and cultural forces
- 5.2 | technological forces | standard | needs: political and legal forces
- 5.3 | interaction | standard | needs: technological forces
- 5.3 | adapting to change | standard | needs: interaction
- 5.3 | influencing the environment | minor | needs: adapting to change
- 5.3 | scanning for change | minor | needs: influencing the environment
defines:
- Environment | All the forces and conditions, inside and outside a business, that affect how it operates.
- Internal environment | Forces within the business that it can control, such as its staff, resources and rules.
- External environment | Forces outside the business, such as customers, laws and prices, that it cannot control directly.
- Stakeholder | Any person or group affected by a business or able to affect it, such as staff or customers.
assumes: Objective, Goodwill, Survival, Plant location, Footfall, Industrial estate
words-in-use: environment; culture
case: Environment record 2025: textbook sales Tk 4,300,000 against Tk 5,100,000 in 2024 after online sellers cut textbook prices by about 15% from March 2025; school curriculum change announced for January 2026; internal: 7 staff, stock listed on paper cards. Overall sales 2025 Tk 14,400,000.
history: Kodak, 1888 to 2012: invented the digital camera in 1975 but filed for bankruptcy protection in January 2012 after failing to adapt | search: Kodak 2012 chapter 11 digital camera 1975
refs: Chapter 3, 'Objectives of Business and What Makes It Succeed', section 3.1 'The Purpose of a Business' — goals the environment can help or block; Chapter 4, 'Choosing Where a Business Stands', section 4.1 'Why Location Matters' — site factors that belong to the external environment
need: you-will-need: Chapter 3 'Objectives of Business and What Makes It Succeed', section 3.1 'The Purpose of a Business'; Chapter 4 'Choosing Where a Business Stands', section 4.1 'Why Location Matters'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: sort ten forces into internal and external; think: respond to a new competitor; pause: after external environment
sources: no

=== CH06 ===
title: Social Responsibility of Business
purpose: Explains what a business owes to society, suppliers, investors and others affected by it.
pace: standard — framework with a moral edge
style: descriptive
words: 5050
figures: 1
- fig01: concentric circles of responsibility from owners outward to society
plate: a Victorian charitable school room with children at long desks and a master
sections:
6.1 | The Idea of Social Responsibility | new idea | 675
6.2 | Responsibility to Society | framework | 675
6.3 | Responsibility to Suppliers | framework | 450
6.4 | Responsibility to Investors and Stakeholders | framework | 650
concepts:
- 6.1 | social responsibility | standard | needs: none
- 6.1 | why it matters | standard | needs: social responsibility
- 6.1 | the case against | standard | needs: why it matters
- 6.2 | duties to community | standard | needs: the case against
- 6.2 | environment and waste | standard | needs: duties to community
- 6.2 | consumers' rights | standard | needs: environment and waste
- 6.3 | fair dealing with suppliers | standard | needs: consumers' rights
- 6.3 | prompt payment | standard | needs: fair dealing with suppliers
- 6.4 | duties to owners and lenders | standard | needs: prompt payment
- 6.4 | duties to employees | standard | needs: duties to owners and lenders
- 6.4 | balancing stakeholders | minor | needs: duties to employees
- 6.4 | recognising a responsible firm | minor | needs: balancing stakeholders
defines:
- Social responsibility | A business's duty to act in ways that benefit society as well as its owners.
- Consumer | A person who uses goods or services, whether or not that person also paid for them.
- Investor | A person who puts money into a business expecting to receive a return.
- Sustainability | Running a business so that it meets today's needs without harming the ability of later generations to meet theirs.
assumes: Objective, Goodwill, Survival, Environment, Internal environment, External environment
words-in-use: responsibility; investment
case: Record 2025: supplies bought from 14 publishers; amounts owed to publishers on 31 December 2025 Tk 1,150,000; agreed payment time 30 days, actual average 47 days; 200 books (retail value Tk 90,000) given to a Kalabagan school library in March 2025; 380 kg of unsold paper sent for recycling; bank loan Tk 600,000.
history: Johnson and Johnson, 1982: after cyanide deaths linked to Tylenol capsules in Chicago, the firm recalled about 31 million bottles | search: Tylenol 1982 recall Johnson and Johnson
refs: Chapter 3, 'Objectives of Business and What Makes It Succeed', section 3.2 'Social and Human Objectives' — social aims; Chapter 5, 'The Business Environment', section 5.2 'Outside the Business' — outside forces
need: you-will-need: Chapter 3 'Objectives of Business and What Makes It Succeed', section 3.2 'Social and Human Objectives'; Chapter 5 'The Business Environment', section 5.2 'Outside the Business'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: match duties to groups; think: a supplier is paid late for months; pause: after the case against
sources: no

=== CH07 ===
title: Forms of Business Ownership and the Sole Proprietorship
purpose: Surveys the ways a business can be owned, then studies the simplest: one owner.
pace: standard — new classification with first example
style: descriptive
words: 4950
figures: 1
- fig01: family tree of ownership forms
plate: a Victorian cobbler alone in his workshop with lasts, leather and a shop sign
sections:
7.1 | Choosing an Ownership Form | new idea | 675
7.2 | The Sole Proprietorship | framework | 900
7.3 | Formation and Management | technique | 775
concepts:
- 7.1 | types of business organisation | standard | needs: none
- 7.1 | private and public sector | standard | needs: types of business organisation
- 7.1 | choosing a form | standard | needs: private and public sector
- 7.2 | sole proprietorship | standard | needs: choosing a form
- 7.2 | features | standard | needs: sole proprietorship
- 7.2 | suitable fields | standard | needs: features
- 7.2 | why it survives | standard | needs: suitable fields
- 7.3 | starting a sole business | standard | needs: why it survives
- 7.3 | licences | minor | needs: starting a sole business
- 7.3 | managing | standard | needs: licences
- 7.3 | limits of unlimited liability | standard | needs: managing
defines:
- Sole proprietorship | A business owned and run by one person, who keeps all profit and bears all losses.
- Unlimited liability | A duty on the owner to pay the business's debts from personal wealth if the business cannot.
- Private sector | The part of the economy owned and run by individuals and private firms rather than government.
- Public sector | The part of the economy owned and run by government.
- Trade licence | Official permission from a local authority to carry on a stated trade in a stated place.
assumes: Business, Profit, Trade, Objective, Goodwill, Survival
words-in-use: liability; sector
case: Status 1 January 2026: sole proprietor Nurul Meghna; capital Tk 4,100,000; debts: amounts owed to publishers Tk 1,150,000 and bank loan Tk 600,000, total Tk 1,750,000; all profit of Tk 750,000 (2025) belongs to him; one trade licence kept current for the shop.
history: Madam C. J. Walker, about 1906: built a hair-care business as sole owner in the United States | search: Madam C. J. Walker company 1906 sole owner
refs: Chapter 1, 'What Business Is', section 1.1 'Why People Do Business' — business activity; Chapter 3, 'Objectives of Business and What Makes It Succeed', section 3.3 'What a Business Needs to Succeed' — what makes a business last
need: you-will-need: Chapter 1 'What Business Is', section 1.1 'Why People Do Business'; Chapter 3 'Objectives of Business and What Makes It Succeed', section 3.3 'What a Business Needs to Succeed'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: list advantages of one-owner business; think: unlimited liability scenario; pause: after unlimited liability
sources: no

=== CH08 ===
title: The Partnership
purpose: Teaches how two or more people can own a business together and what the law expects of them.
pace: standard — framework with legal ideas
style: descriptive
words: 5150
figures: 1
- fig01: partners' capital and profit shares as a divided bar
plate: a Victorian solicitor's office with two men signing a deed, quill and ledger
sections:
8.1 | Idea and Base of Partnership | new idea | 675
8.2 | Kinds of Partner and Partnership | framework | 775
8.3 | Rights, Duties and Dissolution | framework | 900
8.4 | Partnership Compared with Sole Proprietorship | framework | 200
concepts:
- 8.1 | partnership | standard | needs: none
- 8.1 | partnership agreement | standard | needs: partnership
- 8.1 | base of partnership | standard | needs: partnership agreement
- 8.2 | active and sleeping partner | standard | needs: base of partnership
- 8.2 | nominal partner | minor | needs: active and sleeping partner
- 8.2 | limited partner | standard | needs: nominal partner
- 8.2 | partnership at will and for a term | standard | needs: limited partner
- 8.3 | implied duties of partners | standard | needs: partnership at will and for a term
- 8.3 | sharing profit | standard | needs: implied duties of partners
- 8.3 | dissolution | standard | needs: sharing profit
- 8.3 | modes of dissolution | standard | needs: dissolution
- 8.4 | advantages and disadvantages | minor | needs: modes of dissolution
- 8.4 | who should choose which | minor | needs: advantages and disadvantages
defines:
- Partnership | A business owned by two or more people who have agreed to run it together and share its profits.
- Partnership deed | A written agreement setting out partners' capital, duties, profit shares and rules for ending the firm.
- Sleeping partner | A partner who supplies capital and shares profit but takes no active part in running the business.
- Dissolution | The formal ending of a partnership or company, after which it no longer operates as before.
- Mutual agency | The rule that each partner can act for the firm and bind all the other partners.
assumes: Sole proprietorship, Unlimited liability, Private sector
words-in-use: agency; deed
case: Deed dated 1 July 2026: partners Nurul Meghna Tk 2,500,000, Rafiq Meghna Tk 1,000,000, Tariq Meghna Tk 600,000 (total Tk 4,100,000); profit ratio 5:3:2; profit for 1 July to 31 December 2026 Tk 420,000, shared Tk 210,000, Tk 126,000, Tk 84,000.
history: Hewlett and Packard, 1939: Bill Hewlett and David Packard formed a partnership in Palo Alto, California, later incorporated in 1947 | search: Hewlett-Packard partnership 1939 incorporated 1947
refs: Chapter 7, 'Forms of Business Ownership and the Sole Proprietorship', section 7.2 'The Sole Proprietorship' — sole proprietorship from which it grows; Chapter 7, 'Forms of Business Ownership and the Sole Proprietorship', section 7.3 'Formation and Management' — unlimited liability that partners share
need: you-will-need: Chapter 7 'Forms of Business Ownership and the Sole Proprietorship', section 7.2 'The Sole Proprietorship'; Chapter 7 'Forms of Business Ownership and the Sole Proprietorship', section 7.3 'Formation and Management'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: compute profit shares in a new ratio; think: a partner overspends; pause: after mutual agency
sources: no

=== CH09 ===
title: The Joint Stock Company
purpose: Explains the company as a legal person owned by shareholders, and how one is formed.
pace: standard — defining form with many legal terms
style: descriptive
words: 5075
figures: 2
- fig01: steps from idea to commencement as a numbered flow
- fig02: private versus public company drawn as two ownership circles
plate: a Victorian stock exchange floor with brokers, a ticker and top hats
sections:
9.1 | What a Company Is | defining theory | 900
9.2 | Private and Public Companies | framework | 775
9.3 | Forming a Company | technique | 500
9.4 | Is the Company the Best Form? | framework | 300
concepts:
- 9.1 | joint stock company | standard | needs: none
- 9.1 | separate legal person | standard | needs: joint stock company
- 9.1 | limited liability | standard | needs: separate legal person
- 9.1 | share | standard | needs: limited liability
- 9.2 | private limited company | standard | needs: share
- 9.2 | public limited company | standard | needs: private limited company
- 9.2 | distinctive features | standard | needs: public limited company
- 9.2 | merits and demerits | minor | needs: distinctive features
- 9.3 | promoters | minor | needs: merits and demerits
- 9.3 | memorandum of association | minor | needs: promoters
- 9.3 | articles of association | minor | needs: memorandum of association
- 9.3 | prospectus | minor | needs: articles of association
- 9.3 | certificate of commencement | minor | needs: prospectus
- 9.4 | comparing the company | minor | needs: certificate of commencement
- 9.4 | management by directors | minor | needs: comparing the company
- 9.4 | separation of ownership and control | minor | needs: management by directors
defines:
- Joint stock company | A business whose capital is split into shares, owned by shareholders, and which is a legal person separate from them.
- Limited liability | A rule that shareholders lose, at most, the money they put into shares, never their personal wealth.
- Share | One equal part of a company's capital, giving its holder a claim on profit and a voice in decisions.
- Memorandum of association | The founding document stating a company's name, objects, registered office, liability and share capital.
- Articles of association | The document setting the company's internal rules, such as duties of directors and meetings.
- Prospectus | A published invitation to the public to buy shares or debentures, describing the company and the risks.
- Private limited company | A company that restricts transfer of its shares and cannot invite the public to buy them.
- Public limited company | A company that may invite the public to buy its shares, which are often traded on a stock exchange.
assumes: Partnership, Partnership deed, Sleeping partner
words-in-use: share; stock; limited
case: Proposal of 15 September 2026: convert to Meghna and Sons Limited; authorised capital Tk 10,000,000 in 1,000,000 shares of Tk 10; issue 410,000 shares to the partners for existing capital: Nurul 250,000, Rafiq 100,000, Tariq 60,000 shares (total value Tk 4,100,000).
history: Dutch East India Company (VOC), 1602: first company whose shares were openly traded and held by many investors, in Amsterdam | search: VOC 1602 first shares Amsterdam
refs: Chapter 8, 'The Partnership', section 8.4 'Partnership Compared with Sole Proprietorship' — partnership shortcomings that the company answers; Chapter 8, 'The Partnership', section 8.3 'Rights, Duties and Dissolution' — dissolution and liability
need: you-will-need: Chapter 8 'The Partnership', section 8.4 'Partnership Compared with Sole Proprietorship'; Chapter 8 'The Partnership', section 8.3 'Rights, Duties and Dissolution'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: compare two companies; list contents of a prospectus; think: whether a bakery should become a company; pause: after separate legal person
sources: no

=== CH10 ===
title: The Cooperative Society
purpose: Shows how people with a shared need can own a business together on equal terms.
pace: standard — defining idea of mutual ownership
style: descriptive
words: 5175
figures: 1
- fig01: members, committee and general meeting as a ring of authority
plate: a Victorian village store with a counter, shelves of goods and members queuing, Rochdale-style
sections:
10.1 | The Cooperative Idea | defining theory | 675
10.2 | Kinds of Cooperative | framework | 900
10.3 | How a Cooperative Operates | framework | 1000
concepts:
- 10.1 | cooperative society | standard | needs: none
- 10.1 | cooperative principles | standard | needs: cooperative society
- 10.1 | one member one vote | standard | needs: cooperative principles
- 10.2 | consumer cooperative | standard | needs: one member one vote
- 10.2 | producer cooperative | standard | needs: consumer cooperative
- 10.2 | marketing cooperative | standard | needs: producer cooperative
- 10.2 | credit cooperative | standard | needs: marketing cooperative
- 10.3 | formation and registration | standard | needs: credit cooperative
- 10.3 | management committee | standard | needs: formation and registration
- 10.3 | surplus and dividend | standard | needs: management committee
- 10.3 | merits and limits | standard | needs: surplus and dividend
- 10.3 | role in farm marketing | minor | needs: merits and limits
defines:
- Cooperative society | A business owned and run by members who join to meet a common need, each with one vote.
- Cooperative principle | A basic rule of cooperatives, such as open membership, one vote per member and fair sharing of surplus.
- Surplus | In a cooperative, what remains after costs, shared among members instead of paid to outside owners.
- Marketing cooperative | A cooperative that sells its members' products together to obtain better prices.
assumes: Sole proprietorship, Unlimited liability, Private sector, Joint stock company, Limited liability, Share
words-in-use: society; member
case: Proposal 2026: 22 independent Dhaka bookshops form a buying cooperative; Meghna and Sons would hold 10 shares of Tk 1,000 (Tk 10,000); paper and stationery purchases Tk 2,400,000 a year; expected bulk discount 12% = Tk 288,000 a year.
history: Rochdale Equitable Pioneers Society, 1844: 28 weavers opened a store in Toad Lane, Rochdale, England, with rules that became the cooperative principles | search: Rochdale Pioneers 1844 Toad Lane principles
refs: Chapter 7, 'Forms of Business Ownership and the Sole Proprietorship', section 7.1 'Choosing an Ownership Form' — ownership forms; Chapter 9, 'The Joint Stock Company', section 9.1 'What a Company Is' — company as a contrast
need: you-will-need: Chapter 7 'Forms of Business Ownership and the Sole Proprietorship', section 7.1 'Choosing an Ownership Form'; Chapter 9 'The Joint Stock Company', section 9.1 'What a Company Is'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: compare cooperative and company; think: farmers selling jute; pause: after principle
sources: no

=== CH11 ===
title: State Enterprise and Comparing the Forms
purpose: Explains why government runs businesses and sets all ownership forms side by side.
pace: standard — framework then a summary comparison
style: descriptive
words: 5150
figures: 1
- fig01: spectrum from private to public ownership
plate: a Victorian railway station hall with signal box, engines and station master
sections:
11.1 | Why Government Owns Businesses | new idea | 675
11.2 | Forms of State Enterprise | framework | 900
11.3 | Judging State Enterprise | framework | 675
11.4 | Comparing All the Forms | framework | 300
concepts:
- 11.1 | state enterprise | standard | needs: none
- 11.1 | reasons for state ownership | standard | needs: state enterprise
- 11.1 | fields suited to state enterprise | standard | needs: reasons for state ownership
- 11.2 | departmental undertaking | standard | needs: fields suited to state enterprise
- 11.2 | public corporation | standard | needs: departmental undertaking
- 11.2 | government company | standard | needs: public corporation
- 11.2 | control and accountability | standard | needs: government company
- 11.3 | importance | standard | needs: control and accountability
- 11.3 | causes of loss | standard | needs: importance
- 11.3 | case for and against | standard | needs: causes of loss
- 11.4 | comparison by owner, liability, life and control | minor | needs: case for and against
- 11.4 | features of each form | minor | needs: comparison by owner, liability, life and control
- 11.4 | choosing between forms | minor | needs: features of each form
defines:
- State enterprise | A business owned or controlled by government, usually to provide essential goods or services.
- Public corporation | A state enterprise set up by law as a separate body with its own managers and accounts.
- Departmental undertaking | A state enterprise run directly as part of a government department.
- Nationalisation | The transfer of a privately owned business to government ownership.
assumes: Sole proprietorship, Unlimited liability, Private sector, Joint stock company, Limited liability, Share, Cooperative society, Cooperative principle
words-in-use: public; control
case: Supplier comparison 2026: reams of paper from a state-owned press at Tk 520 each against Tk 560 from a private supplier; Meghna and Sons buys 3,000 reams a year, saving Tk 120,000; average delivery 18 days from the state press, 6 days from the private supplier.
history: Tennessee Valley Authority, 1933: created by US law as a public corporation to build dams, supply power and plan development in the Tennessee valley | search: Tennessee Valley Authority 1933 Act
refs: Chapter 7, 'Forms of Business Ownership and the Sole Proprietorship', section 7.1 'Choosing an Ownership Form' — ownership classes; Chapter 9, 'The Joint Stock Company', section 9.1 'What a Company Is' — company contrasted; Chapter 10, 'The Cooperative Society', section 10.1 'The Cooperative Idea' — mutual ownership contrasted
need: you-will-need: Chapter 7 'Forms of Business Ownership and the Sole Proprietorship', section 7.1 'Choosing an Ownership Form'; Chapter 9 'The Joint Stock Company', section 9.1 'What a Company Is'; Chapter 10 'The Cooperative Society', section 10.1 'The Cooperative Idea'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: match activity to suitable ownership form; think: privatise or keep; pause: after types
sources: no

=== CH12 ===
title: Foreign Trade
purpose: Explains why nations trade, how the trade is measured and how it is regulated.
pace: standard — new international ideas
style: descriptive
words: 5075
figures: 1
- fig01: balance of trade shown as two columns
plate: a Victorian sailing ship being loaded at a quay with bales and barrels
sections:
12.1 | Why Nations Trade | new idea | 675
12.2 | Measuring Foreign Trade | framework | 900
12.3 | Regulating Trade | framework | 900
concepts:
- 12.1 | foreign trade | standard | needs: none
- 12.1 | comparative advantage | standard | needs: foreign trade
- 12.1 | gains from trade | standard | needs: comparative advantage
- 12.2 | visible and invisible trade | standard | needs: gains from trade
- 12.2 | balance of trade | standard | needs: visible and invisible trade
- 12.2 | balance of payments | standard | needs: balance of trade
- 12.2 | exchange rate | standard | needs: balance of payments
- 12.3 | tariffs | standard | needs: exchange rate
- 12.3 | quotas | standard | needs: tariffs
- 12.3 | trade agreements and bodies | standard | needs: quotas
- 12.3 | importance of foreign trade to Bangladesh | standard | needs: trade agreements and bodies
defines:
- Foreign trade | The buying and selling of goods and services between people or firms in different countries.
- Export | A good or service sold to a buyer in another country.
- Import | A good or service bought from a seller in another country.
- Balance of trade | The difference in value between a country's exports and imports of goods over a period.
- Balance of payments | A record of all money flowing into and out of a country over a period.
- Tariff | A tax charged on imported goods, which raises their price.
- Exchange rate | The price of one country's money measured in another country's money.
assumes: Business, Profit, Trade, State enterprise, Public corporation, Departmental undertaking
words-in-use: balance; trade
case: Foreign dealings 2026 at an illustrative rate of Tk 120 per US dollar: imported 800 English-language titles worth USD 18,000 (Tk 2,160,000) on 10 March 2026; sold 150 Bangla titles to a Kolkata bookseller worth USD 2,500 (Tk 300,000); the shop's own balance of trade is a deficit of USD 15,500 (Tk 1,860,000).
history: David Ricardo, 1817: in On the Principles of Political Economy, used English cloth and Portuguese wine to show comparative advantage | search: Ricardo 1817 comparative advantage wine cloth
refs: Chapter 1, 'What Business Is', section 1.2 'Trade, Commerce and Industry' — trade defined; Chapter 11, 'State Enterprise and Comparing the Forms', section 11.1 'Why Government Owns Businesses' — state role in business
need: you-will-need: Chapter 1 'What Business Is', section 1.2 'Trade, Commerce and Industry'; Chapter 11 'State Enterprise and Comparing the Forms', section 11.1 'Why Government Owns Businesses'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: classify transactions as visible or invisible; compute a trade balance; think: a tariff on paper; pause: after comparative advantage
sources: no

=== CH13 ===
title: Export and Import Procedure
purpose: Takes the reader through the steps and papers of selling abroad and buying from abroad, and Bangladesh's export promotion.
pace: standard — procedure with many documents
style: descriptive
words: 5125
figures: 2
- fig01: export steps as a ruled flow chart
- fig02: documents that travel with a shipment
plate: a Victorian customs house clerk stamping papers beside crates, dockside
sections:
13.1 | Export Procedure | technique | 900
13.2 | Import Procedure | technique | 675
13.3 | Documents of Foreign Trade | framework | 650
13.4 | Barriers and Promotion | framework | 300
concepts:
- 13.1 | export order and contract | standard | needs: none
- 13.1 | letter of credit | standard | needs: export order and contract
- 13.1 | shipping and clearing | standard | needs: letter of credit
- 13.1 | payment | standard | needs: shipping and clearing
- 13.2 | import licence and registration | standard | needs: payment
- 13.2 | indent and letter of credit | standard | needs: import licence and registration
- 13.2 | customs clearance | standard | needs: indent and letter of credit
- 13.3 | commercial invoice | standard | needs: customs clearance
- 13.3 | bill of lading | standard | needs: commercial invoice
- 13.3 | charter party | minor | needs: bill of lading
- 13.3 | packing list | minor | needs: charter party
- 13.4 | barriers to exports | minor | needs: packing list
- 13.4 | promotion strategies for Bangladesh | minor | needs: barriers to exports
- 13.4 | role of export bodies | minor | needs: promotion strategies for Bangladesh
defines:
- Letter of credit | A bank's written promise to pay an exporter on the buyer's behalf once stated documents are presented.
- Bill of lading | A document from a shipping line that receipts goods, states their carriage terms and shows who may claim them.
- Commercial invoice | The seller's bill listing goods, quantity, price and terms for a shipment.
- Charter party | A contract by which a shipowner hires a whole ship or part of it to a merchant.
- Customs clearance | The official checking of goods at a border and payment of any duty before they are released.
assumes: Foreign trade, Export, Import
words-in-use: document; clearance
case: Export file, 150 Bangla titles, USD 2,500 (Tk 300,000): order from the Kolkata bookseller 2 November 2026; letter of credit opened 5 November 2026; goods packed 12 November; shipped 14 November; documents to bank 15 November; payment received 28 November 2026.
history: Desh Garments, 1978: early Bangladeshi garment exporter, first shipment to Paris, which opened a major export industry | search: Desh Garments 1978 first export Paris
refs: Chapter 12, 'Foreign Trade', section 12.2 'Measuring Foreign Trade' — trade measures; Chapter 12, 'Foreign Trade', section 12.3 'Regulating Trade' — tariffs and regulation
need: you-will-need: Chapter 12 'Foreign Trade', section 12.2 'Measuring Foreign Trade'; Chapter 12 'Foreign Trade', section 12.3 'Regulating Trade'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: order the steps of export; think: a bank refuses a document; pause: after letter of credit
sources: no

=== CH14 ===
title: Multinational Corporations
purpose: Explains the multinational company and its help and harm to a developing host country.
pace: standard — framework with development issues
style: descriptive
words: 4950
figures: 1
- fig01: home country, host country and subsidiaries drawn as a hub
plate: a Victorian merchant's counting house with a globe, ship models and letters from abroad
sections:
14.1 | What a Multinational Is | new idea | 1225
14.2 | Why Multinationals Exist | framework | 450
14.3 | Effects on a Developing Country like Bangladesh | framework | 675
concepts:
- 14.1 | multinational corporation | core | needs: none
- 14.1 | home country and host country | standard | needs: multinational corporation
- 14.1 | parent and subsidiary | standard | needs: home country and host country
- 14.1 | types of manager | standard | needs: parent and subsidiary
- 14.2 | reasons for going abroad | standard | needs: types of manager
- 14.2 | importance to host countries | standard | needs: reasons for going abroad
- 14.3 | benefits | standard | needs: importance to host countries
- 14.3 | problems | standard | needs: benefits
- 14.3 | government control | standard | needs: problems
defines:
- Multinational corporation | A company that owns or controls operations in more than one country, managed from a home country.
- Home country | The country where a multinational company has its headquarters.
- Host country | A country where a multinational company operates through a branch or subsidiary.
- Subsidiary | A company controlled by another company, called the parent, which owns most of its shares.
- Foreign direct investment | Money a foreign company invests to build or buy a lasting business in another country.
assumes: Foreign trade, Export, Import, Joint stock company, Limited liability, Share
words-in-use: control; home
case: Supplier record 2026: Larkspur Academic Press (illustrative; London headquarters, subsidiaries in 9 countries) supplies the shop; trade discount 35% off list; its Dhaka office employs 24 people; an order of 400 books at list GBP 20 at an illustrative Tk 150 per GBP: list value Tk 1,200,000, price after discount Tk 780,000.
history: Singer Manufacturing Company, 1860s to 1880s: early multinational, opening its first overseas factory in Glasgow, Scotland | search: Singer Kilbowie Glasgow factory
refs: Chapter 12, 'Foreign Trade', section 12.1 'Why Nations Trade' — foreign trade basics; Chapter 9, 'The Joint Stock Company', section 9.1 'What a Company Is' — company ownership behind a subsidiary
need: you-will-need: Chapter 12 'Foreign Trade', section 12.1 'Why Nations Trade'; Chapter 9 'The Joint Stock Company', section 9.1 'What a Company Is'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: list host-country gains and losses; think: should a country welcome a foreign firm; pause: after home and host
sources: no

=== CH15 ===
title: Institutions that Promote Business in Bangladesh
purpose: Describes the share market, the regulators and the bodies that help trade and industry in Bangladesh.
pace: slow — many institutions to hold apart
style: descriptive
words: 5175
figures: 1
- fig01: institutions as a map of who regulates, trades, promotes and serves
plate: a Victorian chamber of commerce meeting hall with merchants at a long table
sections:
15.1 | The Share Market | new idea | 900
15.2 | The Regulator and the Exchange | framework | 675
15.3 | Bodies for Trade and Industry | framework | 1000
concepts:
- 15.1 | share market | standard | needs: none
- 15.1 | primary and secondary market | standard | needs: share market
- 15.1 | initial public offering | standard | needs: primary and secondary market
- 15.1 | stock broker | standard | needs: initial public offering
- 15.2 | Bangladesh Securities and Exchange Commission | standard | needs: stock broker
- 15.2 | Dhaka Stock Exchange | standard | needs: Bangladesh Securities and Exchange Commission
- 15.2 | investor protection | standard | needs: Dhaka Stock Exchange
- 15.3 | FBCCI | standard | needs: investor protection
- 15.3 | export processing zone | standard | needs: FBCCI
- 15.3 | Export Promotion Bureau | standard | needs: export processing zone
- 15.3 | TCCB | standard | needs: Export Promotion Bureau
- 15.3 | other institutions | minor | needs: TCCB
defines:
- Initial public offering | The first time a company sells its shares to the public, so that they can later be traded.
- Stock exchange | An organised market where shares and bonds already issued are bought and sold.
- Export processing zone | A planned area with special rules and benefits to attract factories that make goods for export.
- Stock broker | A licensed intermediary who buys and sells shares for clients and earns a commission.
- Regulator | An official body that makes and enforces the rules for an industry or market.
assumes: Joint stock company, Limited liability, Share, Foreign trade, Export, Import
words-in-use: market; exchange
case: Plan of 20 October 2026: Meghna and Sons Limited considers an initial public offering of 200,000 shares at Tk 25 each (face value Tk 10, premium Tk 15), raising Tk 5,000,000; trade body subscription Tk 12,000 a year. Illustrative only.
history: Buttonwood Agreement, 1792: 24 brokers signed it in New York, which led to the New York Stock Exchange; Dhaka's exchange was set up in 1954 | search: Buttonwood Agreement 1792; Dhaka Stock Exchange founded 1954
refs: Chapter 9, 'The Joint Stock Company', section 9.1 'What a Company Is' — company shares; Chapter 9, 'The Joint Stock Company', section 9.3 'Forming a Company' — prospectus; Chapter 12, 'Foreign Trade', section 12.3 'Regulating Trade' — trade rules
need: you-will-need: Chapter 9 'The Joint Stock Company', section 9.1 'What a Company Is'; Chapter 9 'The Joint Stock Company', section 9.3 'Forming a Company'; Chapter 12 'Foreign Trade', section 12.3 'Regulating Trade'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: sort roles among institutions; think: why list a firm; pause: after IPO
sources: no

=== CH16 ===
title: Business Combination and Integration
purpose: Explains why firms join together, the forms of joining and the benefits and dangers.
pace: standard — framework to end on
style: descriptive
words: 5150
figures: 1
- fig01: horizontal and vertical combination as two chains
plate: a Victorian cartel meeting of mill owners around a map and ledger
sections:
16.1 | Why Firms Combine | new idea | 900
16.2 | Forms of Combination | framework | 1125
16.3 | Horizontal and Vertical Integration | framework | 525
concepts:
- 16.1 | business combination | standard | needs: none
- 16.1 | reasons | standard | needs: business combination
- 16.1 | opportunities | standard | needs: reasons
- 16.1 | threats | standard | needs: opportunities
- 16.2 | merger | standard | needs: threats
- 16.2 | acquisition and takeover | standard | needs: merger
- 16.2 | consolidation | standard | needs: acquisition and takeover
- 16.2 | holding company | standard | needs: consolidation
- 16.2 | cartel and trust | standard | needs: holding company
- 16.3 | horizontal integration | standard | needs: cartel and trust
- 16.3 | vertical integration | minor | needs: horizontal integration
- 16.3 | conglomerate | minor | needs: vertical integration
- 16.3 | weighing opportunity and threat | minor | needs: conglomerate
defines:
- Business combination | Two or more businesses joining under common control, in order to gain strength or reduce competition.
- Merger | A combination in which one company absorbs another and only one remains.
- Takeover | Gaining control of a company by buying most of its shares.
- Vertical integration | Combining firms at different stages of making and selling one product, such as printer and bookseller.
- Horizontal integration | Combining firms at the same stage of making or selling the same kind of product.
- Cartel | A group of independent firms that agree to fix prices or output to limit competition.
assumes: Joint stock company, Limited liability, Share, Initial public offering, Stock exchange, Export processing zone, Foreign trade, Export
words-in-use: combination; control
case: Proposal 3 December 2026: acquire neighbouring shop Padma Stationers (illustrative), sales Tk 6,000,000 a year; price Tk 1,800,000; shared customers 40 a week; expected rent saving Tk 360,000 a year; combined 2025 sales would be Tk 20,400,000.
history: Standard Oil Trust, 1882: John D. Rockefeller combined refiners into a trust, and the US Sherman Act of 1890 answered such combinations | search: Standard Oil Trust 1882 Sherman Act 1890
refs: Chapter 9, 'The Joint Stock Company', section 9.1 'What a Company Is' — company ownership and shares; Chapter 15, 'Institutions that Promote Business in Bangladesh', section 15.1 'The Share Market' — share market; Chapter 12, 'Foreign Trade', section 12.1 'Why Nations Trade' — trade ideas
need: you-will-need: Chapter 9 'The Joint Stock Company', section 9.1 'What a Company Is'; Chapter 15 'Institutions that Promote Business in Bangladesh', section 15.1 'The Share Market'; Chapter 12 'Foreign Trade', section 12.1 'Why Nations Trade'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: classify combinations; think: a baker buying a flour mill; pause: after forms
sources: no
