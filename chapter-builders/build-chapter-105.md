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
course: MGT 105
book: Foundations of Financial Management
spelling: British · dates: day, month, year (12 March 2026) · country: mixed, no single country; running case in Bangladesh; name the place when law or standards differ · currency and numbers: taka for the running case, written Tk 158,000 (international digit grouping); a historical case keeps its own currency
running case: Meghna and Sons, a family-run bookshop in Dhaka — illustrative. Base facts (FIXED): founded 12 January 1998 by Abdur Rahim Chowdhury; since 2019 run by his sons Imran (shop and buying) and Tanvir (accounts and money); at 1 January 2024 assets Tk 2,400,000 (stock Tk 1,850,000, cash and bank Tk 240,000, fittings Tk 310,000), bank loan Tk 600,000 at 10% a year, family equity Tk 1,800,000, sales for 2023 Tk 7,200,000; registered as Meghna and Sons Limited on 1 July 2025; tax rate assumed at 25% for illustration only.
chosen words (one term, one word):
- use business — also called firm, company, enterprise (the named case keeps its own name)
- use common stock — also called ordinary shares, equity shares
- use preferred stock — also called preference shares
- use required return — also called required rate of return, hurdle rate
- use discount rate — also called the rate used for discounting
- use financial manager — also called finance manager, chief financial officer
- use interest rate — also called rate of interest
- use profit — also called earnings or income, except in the fixed terms earnings per share and retained earnings
- use wealth maximisation — also called value maximisation
- use bond — also called a debt security; a debenture is one kind of bond (Chapter 17)
- use return — also called yield, except in the fixed term yield to maturity
- use EBIT after its first definition in Chapter 7 — also called operating profit
size plan: 18 chapters plus appendix A1, ~93,525 words, ~361 pages (size 360 pages, tolerance 6 pages)
chapters:
1. Money, Finance and the Firm
2. The Financial Manager's Work
3. The Goal of the Firm and Agency
4. The Time Value of Money: Single Sums
5. Annuities
6. Uneven Cash Flows
7. Capital Budgeting: Decisions and Cash Flows
8. Capital Budgeting: Evaluation Techniques
9. Return and Risk
10. Risk, Required Return and Market Efficiency
11. Operating Leverage and Break-even Analysis
12. Financial and Total Leverage
13. Valuing Bonds
14. Valuing Common and Preferred Stock
15. Working Capital and the Cash Cycle
16. Short-Term Financing and Matching
17. Long-Term Financing and the Cost of Capital
18. Leasing
A1. Maths You Will Use

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
title: Money, Finance and the Firm
purpose: Introduce finance, its place in a business, and the flow of funds between owners, lenders and the business.
pace: slow — first chapter; teaches how to read the book as well as the subject
style: descriptive
words: 5,000
figures: 1
- fig01: flow of funds between owners, lenders, the shop and its assets, with arrows labelled by plain words
plate: a money-changer at his counter with coin scales, stacked coins and an open ledger, 1850s
sections:
1.1 | What Finance Is | new idea | 875
1.2 | Finance Among the Business Functions | framework | 675
1.3 | Where Funds Come From and Where They Go | framework | 850
concepts:
- 1.1 | Finance | core | needs: none
- 1.1 | Funds and cash flow | standard | needs: none
- 1.1 | Wealth | minor | needs: Funds
- 1.2 | Business function | standard | needs: none
- 1.2 | Finance and management | standard | needs: Finance, Business function
- 1.2 | Finance beside accounting and economics | standard | needs: Finance
- 1.3 | Source of funds | standard | needs: Funds
- 1.3 | Use of funds and assets | standard | needs: Funds
- 1.3 | Equity | minor | needs: Source of funds
- 1.3 | Debt | minor | needs: Source of funds
- 1.3 | Liability | minor | needs: Debt
- 1.3 | Financial market | minor | needs: Source of funds
defines:
- Finance | the work of finding money and deciding how to use it, so a business or person can reach chosen aims
- Funds | money that a business has available to spend, save or invest
- Cash flow | money moving into or out of a business during a period
- Wealth | the total value of what a person or business owns, after taking away what is owed
- Business function | one main kind of work in a business, such as selling, making or managing money
- Source of funds | a place a business gets money from, such as its owners or a lender
- Use of funds | a purpose a business spends money on, such as stock or equipment
- Asset | something of value that a business owns and uses to earn money
- Equity | money put in by the owners, who share the business profit or loss
- Debt | money borrowed that must be repaid, with an extra charge for using it
- Liability | an amount a business owes to someone else
- Financial market | a place or network where buyers and sellers trade money, loans and ownership parts of businesses
assumes: none
words-in-use: profit, capital, account
case: Meghna and Sons, Dhaka. At 1 January 2024 the shop holds stock Tk 1,850,000, cash and bank balance Tk 240,000 and fittings Tk 310,000 (assets Tk 2,400,000). Funds came from a bank loan of Tk 600,000 at 10% a year (debt) and the family's own funds of Tk 1,800,000 (equity). Sales in 2023 were Tk 7,200,000. Founded 12 January 1998 by Abdur Rahim Chowdhury; since 2019 his sons Imran (shop and buying) and Tanvir (accounts and money) run it.
history: Medici Bank, Florence, founded 1397 by Giovanni di Bicci de Medici: bills of exchange let merchants move money between cities without carrying coin. search: Medici Bank 1397 bill of exchange history
refs: none
need: nothing
ladder: familiar case (a market-stall seller and her cash); reshaped case (the same seller takes a loan from a cousin); new setting (a school canteen run by a committee); combined case (a small clinic that must name its sources and uses of funds and say where finance sits among its functions)
practice: review: meaning of finance for a stall owner; finance versus accounting for a tailor; sources and uses of funds for a new bakery funded by savings and a loan; why cash flow differs from profit when a customer pays next month; think: a cinema owner takes Tk 40,000 in ticket sales, owes Tk 25,000 for film hire next month and pays a Tk 10,000 loan instalment — classify each movement and say which are cash flows today; pause: after Funds and cash flow; after Equity and Debt
sources: no

=== CH02 ===
title: The Financial Manager's Work
purpose: Show who does finance in a business, what three decisions they take, and how finance shapes day-to-day success.
pace: slow — three similar-sounding decisions must be kept apart
style: descriptive
words: 4,900
figures: 1
- fig01: the three decisions drawn as three labelled arrows around a box of funds
plate: a counting-house master at a standing desk reviewing papers while two clerks write in tall ledgers
sections:
2.1 | The Financial Manager's Role | framework | 775
2.2 | The Three Core Decisions | framework | 1100
2.3 | How Finance Shapes Business Success | framework | 425
concepts:
- 2.1 | Financial manager | core | needs: Finance, Business function
- 2.1 | Financial planning and control | standard | needs: Funds
- 2.2 | Investment decision | core | needs: Use of funds, Asset
- 2.2 | Financing decision | standard | needs: Source of funds, Equity, Debt
- 2.2 | Dividend decision | standard | needs: Equity
- 2.2 | Dividend and retained earnings | minor | needs: Equity
- 2.3 | Operating efficiency | standard | needs: Use of funds
- 2.3 | Liquidity | minor | needs: Cash flow
- 2.3 | Profitability | minor | needs: Asset
defines:
- Financial manager | the person who plans, raises and controls the money of a business
- Financial planning | deciding in advance how much money a business will need and when
- Financial control | checking that money is spent and recorded as planned, and acting when it is not
- Investment decision | choosing which long-lasting assets or projects a business should buy
- Financing decision | choosing how to raise the money for purchases, from owners or lenders
- Dividend decision | choosing how much profit to pay out to owners and how much to keep
- Dividend | a share of profit paid out to the owners of a business
- Retained earnings | profit kept in the business instead of being paid out to owners
- Operating efficiency | how little money and effort a business uses to produce each unit of sales
- Liquidity | how easily a business can pay its bills on time from cash it has or can quickly raise
- Profitability | how much profit a business earns compared with the money invested or the sales made
assumes: Finance, Funds, Business function, Source of funds, Use of funds, Asset, Equity, Debt, Cash flow
words-in-use: profit, stock
case: Meghna and Sons, 10 February 2024. Profit after tax for 2023: Tk 480,000. Family withdrawal (dividend) Tk 120,000, so retained earnings Tk 360,000. Decision on 10 February 2024: open a stationery corner costing Tk 350,000 (investment decision), funded by Tk 150,000 of retained earnings and a new bank loan of Tk 200,000 at 10% a year (financing decision); the other Tk 210,000 of retained earnings buys textbook stock before the new term. Bank loan after the new borrowing: Tk 800,000. Yearly interest on the new loan: Tk 20,000.
history: Barings Bank, London, collapsed 26 February 1995 after unchecked trading losses in Singapore; weak financial control, not bad luck, was the cause. search: Barings Bank collapse 1995 Nick Leeson control failure
refs: Chapter 1, 'Money, Finance and the Firm', section 1.3 'Where Funds Come From and Where They Go' — sources and uses of funds are the raw material of the three decisions; Chapter 1, 'Money, Finance and the Firm', section 1.2 'Finance Among the Business Functions' — where finance sits among the business functions
need: Chapter 1, section 1.3: sources and uses of funds
ladder: familiar case (a household deciding between a new fridge and repaying a loan); reshaped case (the same choice for a corner shop); new setting (a clinic buying a scanner); combined case (a small manufacturer facing all three decisions in one year)
practice: review: roles of a financial manager in a small firm; the three decisions applied to a clinic that buys a scanner; planning versus control; how weak liquidity disrupts daily operations; think: a bakery has Tk 150,000 of spare profit and must weigh a new oven, a payout to owners, or repaying a loan — name the decision type in each option; pause: after the investment and financing decisions are separated
sources: no

=== CH03 ===
title: The Goal of the Firm and Agency
purpose: Explain why wealth, not profit alone, is the goal, and why hired managers may not pursue it.
pace: slow — agency is new and abstract
style: descriptive
words: 5,125
figures: 1
- fig01: owner, hired manager and the gap between their aims, drawn as two arrows with a labelled space between
plate: a crowded shareholders meeting in a long hall, a chairman at a table and reporters writing on the side
sections:
3.1 | Profit Maximisation and Its Limits | framework | 775
3.2 | Wealth Maximisation | framework | 750
3.3 | Agency Theory | defining theory | 1000
concepts:
- 3.1 | Profit maximisation | core | needs: Profitability
- 3.1 | Timing and risk of profit | standard | needs: Profit maximisation
- 3.2 | Wealth maximisation | core | needs: Wealth, Profit maximisation
- 3.2 | Market value | minor | needs: Wealth
- 3.2 | Stakeholder | minor | needs: none
- 3.3 | Principal and agent | standard | needs: none
- 3.3 | Agency conflict | core | needs: Principal and agent, Wealth maximisation
- 3.3 | Agency cost | standard | needs: Agency conflict
defines:
- Profit maximisation | the aim of earning the largest profit possible in a period, usually one year
- Wealth maximisation | the aim of making the market value of the owners stake as large as possible over time
- Market value | the price at which something could be sold today in a market
- Stakeholder | any person or group affected by a business, such as workers, customers, lenders and owners
- Principal | a person who hires another person to act for them
- Agent | a person who acts for a principal and takes decisions in the principal name
- Agency conflict | a clash between what a principal wants and what an agent chooses to do
- Agency cost | the loss in value, plus the cost of prevention, that comes from an agency conflict
assumes: Wealth, Profitability, Dividend, Financial manager, Retained earnings
words-in-use: value, interest
case: Meghna and Sons, 1 April 2024. A second branch is opened and Mr Shafiq is hired to run it, paid 5% of branch sales as a yearly bonus. Plan X (steady pricing): sales Tk 1,500,000, profit before bonus Tk 150,000, bonus Tk 75,000, owners keep Tk 75,000. Plan Y (deep discounts and credit to schools): sales Tk 2,000,000, profit before bonus Tk 70,000, bonus Tk 100,000, owners keep minus Tk 30,000. The manager gains Tk 25,000 more under Y; the owners lose Tk 105,000. On 15 May 2024 a second choice: cutting restocking raises this year profit by Tk 80,000 but lowers next year profit by Tk 150,000.
history: Adam Smith, The Wealth of Nations (1776), on directors who manage other peoples money; Jensen and Meckling, "Theory of the Firm" (1976), which named agency cost. search: Jensen Meckling 1976 theory of the firm agency costs
refs: Chapter 2, 'The Financial Manager's Work', section 2.2 'The Three Core Decisions' — dividend and financing decisions show what owners give up or gain; Chapter 1, 'Money, Finance and the Firm', section 1.1 'What Finance Is' — wealth was first defined as a stock of value
need: Chapter 2, section 2.3 Profitability; Chapter 1, section 1.1 Wealth
ladder: familiar case (a tailor who skips fabric restocking to show a bigger monthly profit); reshaped case (a hired driver paid per trip); new setting (a school principal and a bursar); combined case (a hired manager, a lender and the owners with three different aims)
practice: review: why profit maximisation misleads when returns come at different times or carry different risk; wealth maximisation explained to a village shopkeeper; an agency conflict between a taxi owner and a driver paid per trip; ways of aligning interests; think: a manager paid on sales chooses between two plans — decide which each side prefers and compute the agency cost from new figures; pause: after wealth maximisation is stated
sources: yes

=== CH04 ===
title: The Time Value of Money: Single Sums
purpose: Teach why a taka today differs from a taka later, and how to move one amount forward and back in time.
pace: slow — compounding and discounting are new and abstract
style: calculation
words: 6,025
figures: 2
- fig01: growth of Tk 200,000 at 8% over five years, simple interest against compound interest, two lines
- fig02: a timeline with an arrow forward labelled compounding and an arrow back labelled discounting
plate: a pocket watch beside a stack of coins and a small plant growing from a pot of soil, as a still life
sections:
4.1 | Why Time Changes the Value of Money | new idea | 1000
4.2 | Future Value of a Single Sum | technique | 975
4.3 | Present Value of a Single Sum | technique | 1000
4.4 | Compounding Within the Year | technique | 450
concepts:
- 4.1 | Time value of money | core | needs: Funds
- 4.1 | Interest and interest rate | standard | needs: Debt
- 4.1 | Opportunity cost | standard | needs: none
- 4.2 | Simple interest | minor | needs: Interest and interest rate
- 4.2 | Compound interest and future value | core | needs: Simple interest
- 4.2 | Single sum | minor | needs: none
- 4.2 | Future value factor | standard | needs: Compound interest and future value
- 4.3 | Present value and discounting | core | needs: Future value
- 4.3 | Discount rate | standard | needs: Present value and discounting
- 4.3 | Using factor tables | standard | needs: Future value factor
- 4.4 | Compounding period | standard | needs: Compound interest and future value
- 4.4 | Effective annual rate | standard | needs: Compounding period
defines:
- Time value of money | the idea that money received sooner is worth more than the same amount received later
- Interest | the price paid for using money for a period, received by the lender
- Interest rate | the interest for one period, shown as a percentage of the amount lent or borrowed
- Opportunity cost | the value of the best choice given up when you pick another
- Simple interest | interest worked out on the original amount only
- Compound interest | interest worked out on the original amount plus interest already earned
- Single sum | one payment or receipt made at one point in time
- Future value | what an amount today will grow to after a stated time at a stated rate
- Present value | what an amount to be received later is worth today at a stated rate
- Discounting | finding a present value by working backwards from a future amount at a rate
- Discount rate | the rate used to turn a future amount into a present value
- Compounding period | the time between one addition of interest and the next
- Effective annual rate | the single yearly rate that gives the same growth as compounding within the year
assumes: Funds, Debt, Equity, Cash flow, Wealth
words-in-use: interest, present, rate
case: Meghna and Sons, 1 July 2024. Tk 200,000 is deposited at 8% a year for 5 years. Simple interest gives Tk 200,000 x (1 + 0.08 x 5) = Tk 280,000. Compound interest gives Tk 200,000 x 1.08^5 = Tk 293,865.62 (factor 1.469328). Tk 500,000 is needed on 30 June 2029 for a premises deposit: present value at 8% = Tk 340,291.60 (factor 0.680583). Compounded half-yearly at 4% for 10 periods, Tk 200,000 grows to Tk 296,048.86; the effective annual rate is 8.16%.
history: Jacob Bernoulli, 1683, studied continuous compounding while asking what happens if interest is added more and more often; his limit became the number e. search: Jacob Bernoulli 1683 compound interest origin of e
refs: Chapter 1, 'Money, Finance and the Firm', section 1.1 'What Finance Is' — funds and wealth define what is being timed; Chapter 3, 'The Goal of the Firm and Agency', section 3.2 'Wealth Maximisation' — why a manager prefers earlier wealth
need: Chapter 1, section 1.1: funds and cash flow
ladder: clean numbers (Tk 10,000 for 2 years at 10%); realistic numbers (Tk 36,500 for 6 years at 7.5%); a missing-rate or missing-time problem solved from factor tables; a combined problem comparing two offers with different times and rates
practice: review: future value of Tk 15,000 at 9% for 6 years; present value of Tk 80,000 due in 4 years at 11%; simple against compound interest on Tk 25,000 over 8 years at 6%; effective annual rate for 12% compounded quarterly; opportunity cost of idle cash; think: a student chooses between Tk 90,000 today and Tk 130,000 in 5 years when money can earn 7% — decide and show the check; pause: after compound interest; after discounting
sources: yes

=== CH05 ===
title: Annuities
purpose: Teach equal payments over time: their future and present values, and how to find an unknown payment.
pace: slow — a series of payments needs a new way of seeing time
style: calculation
words: 5,700
figures: 2
- fig01: two timelines, payments at period ends and payments at period starts
- fig02: stacked bars of deposits and interest making up the future value of an annuity
plate: a row of small savings-bank deposit boxes, and a clerk stamping a passbook at a brass-barred counter
sections:
5.1 | What an Annuity Is | new idea | 875
5.2 | Future Value of an Annuity | technique | 775
5.3 | Present Value of an Annuity | technique | 1000
5.4 | Finding the Payment | technique | 450
concepts:
- 5.1 | Annuity | core | needs: Single sum
- 5.1 | Ordinary annuity and annuity due | standard | needs: Annuity
- 5.1 | Perpetuity | minor | needs: Annuity
- 5.2 | Future value of an ordinary annuity | core | needs: Future value, Annuity
- 5.2 | Future value of an annuity due | standard | needs: Ordinary annuity and annuity due
- 5.3 | Present value of an ordinary annuity | core | needs: Present value, Annuity
- 5.3 | Present value of an annuity due and a perpetuity | standard | needs: Perpetuity
- 5.3 | Annuity factor tables | standard | needs: Factor table
- 5.4 | Equal deposits to reach a target | standard | needs: Future value of an ordinary annuity
- 5.4 | Loan payments | standard | needs: Present value of an ordinary annuity
defines:
- Annuity | a series of equal payments made at equal time intervals for a set number of periods
- Ordinary annuity | an annuity whose payments fall at the end of each period
- Annuity due | an annuity whose payments fall at the start of each period
- Perpetuity | an annuity that never ends
- Sinking fund | money put aside in equal deposits to reach a target sum by a set date
- Amortisation | repaying a loan by equal payments that cover the interest and part of the amount borrowed
- Factor table | a printed table of ready-made multipliers for given rates and numbers of periods
assumes: Future value, Present value, Discount rate, Compound interest, Interest rate, Single sum, Time value of money
words-in-use: annuity, due
case: Meghna and Sons, 15 August 2024. All at 8% a year. Tanvir deposits Tk 60,000 at the end of each of 5 years: future value Tk 351,996.06 (factor 5.866601); if deposited at the start of each year, Tk 380,155.74. Landlord offers 4 yearly rent payments of Tk 120,000 at year end: present value Tk 397,455.22 (factor 3.312127). A van replacement needs Tk 500,000 in 5 years: equal year-end deposit Tk 85,228.23. A scholarship fund paying Tk 50,000 a year for ever has present value Tk 625,000.
history: Edmond Halley, 1693, built a table of death rates from the records of Breslau so that life annuities could be priced fairly. search: Halley 1693 Breslau mortality table annuity pricing
refs: Chapter 4, 'The Time Value of Money: Single Sums', section 4.2 'Future Value of a Single Sum' — future value of a single sum is the building block; Chapter 4, 'The Time Value of Money: Single Sums', section 4.3 'Present Value of a Single Sum' — present value of a single sum is the building block
need: Chapter 4, sections 4.2 and 4.3: future and present value of a single sum
ladder: clean numbers (Tk 1,000 a year for 3 years at 10%); realistic numbers (Tk 60,000 a year for 5 years at 8%); an annuity due set beside an ordinary annuity; a combined problem that finds an unknown payment and checks it by future value
practice: review: future value of Tk 12,000 deposited yearly for 6 years at 9% as ordinary annuity and as annuity due; present value of a Tk 45,000 yearly payment for 5 years at 10%; yearly deposit that reaches Tk 300,000 in 4 years at 7%; level yearly loan payment on Tk 250,000 over 3 years at 12%; think: choose between a lump sum offer and a 5-year payment plan using present values; pause: after the ordinary annuity formula; after annuity due
sources: no

=== CH06 ===
title: Uneven Cash Flows
purpose: Extend present and future value to payments that change in size, and compare streams at a common rate.
pace: standard — builds directly on Chapters 4 and 5
style: calculation
words: 5,025
figures: 1
- fig01: paired bars of yearly receipts for two five-year streams, one rising and one falling, with a label for each year
plate: a lawyers office with deed boxes marked by year, an hourglass and a clerk sealing a document
sections:
6.1 | What a Mixed Stream Is | new idea | 650
6.2 | Present and Future Value of a Mixed Stream | technique | 1000
6.3 | Comparing Streams with a Common Rate | technique | 775
concepts:
- 6.1 | Mixed stream | core | needs: Annuity, Single sum
- 6.1 | Single sum, annuity and mixed stream compared | minor | needs: Mixed stream
- 6.2 | Present value of a mixed stream | core | needs: Present value, Discounting
- 6.2 | Future value of a mixed stream | standard | needs: Future value
- 6.2 | Streams that contain an annuity part | standard | needs: Annuity
- 6.3 | Comparing streams | core | needs: Present value of a mixed stream
- 6.3 | Testing the discount rate | standard | needs: Discount rate
defines:
- Mixed stream | a series of payments or receipts that differ in size from one period to the next
- Discounted cash flow | a way of valuing a series of future cash flows by turning each one into a present value
assumes: Present value, Future value, Discount rate, Discounting, Annuity, Single sum, Cash flow, Time value of money
words-in-use: stream
case: Meghna and Sons, 10 October 2024. Two book-fair stall plans, receipts at year end for 5 years. Stream A: Tk 40,000; 55,000; 60,000; 70,000; 80,000. Stream B: Tk 80,000; 70,000; 60,000; 55,000; 40,000. Both total Tk 305,000. At 12%: present value of A Tk 212,147.18, of B Tk 227,589.53; future value at year 5 of A Tk 373,875.81, of B Tk 401,090.51. At 12% B is worth more because larger receipts come sooner.
history: Benjamin Franklin, who died in 1790, left Boston and Philadelphia 1,000 pounds each to be lent out for 200 years, with fixed amounts withdrawn at 100 years. search: Benjamin Franklin bequest 1790 Boston Philadelphia trust 200 years
refs: Chapter 5, 'Annuities', section 5.3 'Present Value of an Annuity' — annuity present value is used for any level part of a stream; Chapter 4, 'The Time Value of Money: Single Sums', section 4.3 'Present Value of a Single Sum' — each year receipt is discounted as a single sum
need: Chapter 5, section 5.3; Chapter 4, section 4.3
ladder: clean numbers (three yearly receipts of Tk 1,000, 2,000 and 3,000 at 10%); realistic numbers (five receipts at 12%); a stream with a level part and an odd part; a combined comparison of two streams with the same total at three different rates
practice: review: present value of receipts of Tk 20,000, 35,000 and 50,000 over three years at 10%; future value of the same stream at the end of year 3; split a stream into an annuity part and an odd part; compare a rising and a falling stream at 9% and 15%; think: which of two supplier payment plans costs less in present value terms; pause: after the first mixed stream is valued
sources: no

=== CH07 ===
title: Capital Budgeting: Decisions and Cash Flows
purpose: Explain long-term investment decisions, the steps to take, and how to measure the cash flows of a project.
pace: standard — new vocabulary but concrete cases
style: mixed
words: 5,350
figures: 1
- fig01: flow chart of the steps of the capital budgeting process, boxes joined by arrows with plain-word labels
plate: a railway engineer unrolling a survey plan at a bridge site while workmen stand beside a half-built pier
sections:
7.1 | What Capital Budgeting Is and Why It Matters | new idea | 875
7.2 | The Steps of the Process | framework | 650
7.3 | Cash Flow from an Investment | technique | 1225
concepts:
- 7.1 | Capital budgeting | core | needs: Investment decision
- 7.1 | Capital expenditure | minor | needs: Asset
- 7.1 | Why careful appraisal matters | standard | needs: Capital budgeting
- 7.2 | The capital budgeting process | core | needs: Capital budgeting
- 7.2 | Independent and mutually exclusive projects | minor | needs: Capital budgeting
- 7.3 | Relevant cash flow | standard | needs: Cash flow
- 7.3 | Initial investment | standard | needs: Relevant cash flow
- 7.3 | Operating cash flow | core | needs: Depreciation, EBIT
- 7.3 | Terminal cash flow | standard | needs: Initial investment
defines:
- Capital budgeting | the process of planning, evaluating and choosing long-term investments
- Capital expenditure | spending on assets that will serve the business for many years
- Independent projects | projects where accepting one has no effect on whether another is accepted
- Mutually exclusive projects | projects where accepting one means rejecting the others
- Relevant cash flow | cash flow that changes only because a project is accepted
- Initial investment | cash spent at the start to get a project going
- Operating cash flow | cash a project produces each year from running it, after tax
- Terminal cash flow | cash received at the end of a project life, such as the resale value of equipment
- Depreciation | the part of an asset cost charged as an expense in each year of its use
- EBIT | earnings before interest and tax: the profit from operations before paying lenders or the tax office
assumes: Investment decision, Cash flow, Asset, Use of funds, Time value of money
words-in-use: capital, project, cost
case: Meghna and Sons, 5 January 2025: a print-on-demand machine. Initial investment Tk 900,000: machine Tk 780,000, installation Tk 60,000 (both depreciable, Tk 840,000), working capital Tk 60,000. A Tk 15,000 feasibility survey paid in December 2024 is a sunk cost and not relevant. Each year: added revenue Tk 700,000, cash costs Tk 380,000, depreciation Tk 144,000 (straight line over 5 years), EBIT Tk 176,000, tax at 25% (assumed for illustration) Tk 44,000, operating cash flow Tk 276,000 (Tk 132,000 after-tax EBIT plus Tk 144,000 depreciation). End of year 5: resale Tk 120,000 (equal to book value, so no tax on sale) plus working capital Tk 60,000 back: terminal cash flow Tk 180,000, so year 5 total Tk 456,000. A rival plan, a branch fit-out, needs Tk 900,000 and cannot be run together with the machine (mutually exclusive).
history: Padma Bridge, Bangladesh, opened 25 June 2022 and paid for from the national budget rather than foreign loans; a project judged on long-run cash and benefit. search: Padma Bridge opened 25 June 2022 cost financing own funds
refs: Chapter 2, 'The Financial Manager's Work', section 2.2 'The Three Core Decisions' — the investment decision is the decision capital budgeting makes in detail; Chapter 5, 'Annuities', section 5.3 'Present Value of an Annuity' — present value of level cash flows is used in the next chapter
need: Chapter 2, section 2.2: investment decision
ladder: familiar case (a rickshaw owner thinking about a new rickshaw); reshaped case (the same decision with a loan); new setting (a clinic choosing equipment); combined case (a shop comparing two projects and listing every relevant cash flow for each)
practice: review: what capital budgeting is and why a firm needs it; steps in the process applied to a bakery buying a second oven; independent against mutually exclusive projects with two examples; which of five listed cash items are relevant and why; think: build the cash flow table of a delivery van project from new figures; pause: after relevant cash flow
sources: no

=== CH08 ===
title: Capital Budgeting: Evaluation Techniques
purpose: Teach payback and net present value in full, with a first look at internal rate of return and the profitability index.
pace: standard — each technique is one clear procedure
style: calculation
words: 4,675
figures: 2
- fig01: a cumulative cash flow line that crosses zero at the payback point
- fig02: net present value plotted against discount rate for one project, crossing zero at the internal rate of return
plate: a merchant weighing two cargo manifests on a brass balance, with two ships visible through the window behind him
sections:
8.1 | Payback Period | technique | 650
8.2 | Net Present Value | technique | 875
8.3 | Internal Rate of Return and Profitability Index | technique | 325
8.4 | When the Methods Disagree | framework | 225
concepts:
- 8.1 | Payback period | core | needs: Cash flow
- 8.1 | Limits of payback | minor | needs: Payback period
- 8.2 | Net present value | core | needs: Present value, Initial investment
- 8.2 | Required return and the decision rule | standard | needs: Net present value
- 8.2 | Choosing between projects by net present value | minor | needs: Mutually exclusive projects
- 8.3 | Internal rate of return | standard | needs: Net present value
- 8.3 | Profitability index | minor | needs: Net present value
- 8.4 | Conflicting rankings | standard | needs: Payback period, Net present value
defines:
- Payback period | the time a project takes to recover its initial investment from its cash flows
- Net present value | the present value of a project cash inflows minus its initial investment
- Required return | the lowest return a project must earn to be worth accepting
- Internal rate of return | the discount rate at which a project net present value is exactly zero
- Profitability index | the present value of a project inflows divided by its initial investment
assumes: Capital budgeting, Initial investment, Operating cash flow, Terminal cash flow, Present value, Discount rate, Mutually exclusive projects, Annuity
words-in-use: return, rate
case: Meghna and Sons, 2 February 2025. Required return 12%. Project Press: outlay Tk 900,000; inflows Tk 276,000 in years 1 to 4 and Tk 456,000 in year 5. Project Fit-out: outlay Tk 900,000; inflows Tk 400,000; 300,000; 250,000; 200,000; 150,000. Payback: Press 3.26 years, Fit-out 2.80 years. Net present value at 12%: Press Tk 197,055, Fit-out Tk 86,464. Internal rate of return: Press 19.9%, Fit-out 16.6%. Profitability index: Press 1.22, Fit-out 1.10. Payback prefers Fit-out; net present value prefers Press.
history: Irving Fisher, The Theory of Interest (1930), set out the idea behind net present value; Joel Dean, Capital Budgeting (1951), carried discounted cash flow into company practice. search: Joel Dean Capital Budgeting 1951 discounted cash flow
refs: Chapter 7, 'Capital Budgeting: Decisions and Cash Flows', section 7.3 'Cash Flow from an Investment' — the cash flows used here are built there; Chapter 6, 'Uneven Cash Flows', section 6.2 'Present and Future Value of a Mixed Stream' — a mixed stream is valued by discounting each receipt
need: Chapter 7, section 7.3; Chapter 6, section 6.2
ladder: clean numbers (outlay Tk 10,000, three equal inflows); realistic numbers (five unequal inflows at 12%); two mutually exclusive projects ranked by different methods; a combined problem where methods disagree and the reader must justify a choice
practice: review: payback for a Tk 400,000 project with inflows of 120,000, 150,000, 180,000 and 200,000; net present value of the same project at 10%; accept-or-reject rule; find a project internal rate of return by trial between two rates; profitability index of two projects; think: two projects with the same outlay rank differently by payback and net present value — decide and explain; pause: after payback; after net present value
sources: yes

=== CH09 ===
title: Return and Risk
purpose: Define return and risk, separate the kinds of risk, and measure risk with expected value, standard deviation and the coefficient of variation.
pace: slow — spread and probability are new and abstract
style: mixed
words: 5,350
figures: 2
- fig01: bar chart of the probability distributions of two product lines, three bars each
- fig02: two bell-shaped outlines, one narrow and one wide, over the same expected value
plate: an insurance underwriter at a long table with a model ship, a storm chart and a pair of scales
sections:
9.1 | What Return Is | new idea | 450
9.2 | What Risk Is | new idea | 750
9.3 | Describing Uncertain Outcomes | technique | 775
9.4 | Measuring the Spread of Outcomes | technique | 775
concepts:
- 9.1 | Return | standard | needs: Funds
- 9.1 | Rate of return | standard | needs: Return
- 9.2 | Risk | core | needs: Return
- 9.2 | Business risk | minor | needs: Risk
- 9.2 | Financial risk | minor | needs: Risk, Debt
- 9.3 | Probability and probability distribution | standard | needs: none
- 9.3 | Expected value | core | needs: Probability distribution
- 9.4 | Standard deviation | core | needs: Expected value
- 9.4 | Coefficient of variation | standard | needs: Standard deviation, Expected value
defines:
- Return | the total gain or loss from an investment over a period, income plus any change in value
- Rate of return | the return shown as a percentage of the amount first invested
- Risk | the chance that actual results differ from those expected, especially for the worse
- Business risk | the risk that operating profit varies because of sales, costs and trade conditions
- Financial risk | the extra risk to owners that comes from funding part of a business by borrowing
- Probability | a number from 0 to 1 showing how likely an outcome is
- Probability distribution | a list of possible outcomes with the probability of each, adding up to one
- Expected value | the average outcome, weighting each outcome by its probability
- Standard deviation | a measure of how widely outcomes spread around the expected value
- Coefficient of variation | the standard deviation divided by the expected value, showing risk for each unit of return
assumes: Funds, Debt, Profitability, Net present value, Required return, Cash flow
words-in-use: risk, return, spread
case: Meghna and Sons, 2 March 2025. Return example: a savings bond bought for Tk 100,000 pays Tk 6,000 interest in the year and could be sold at Tk 104,000: return Tk 10,000, rate of return 10%. Two stock lines on Tk 100,000 invested, with probabilities 0.25 (slow), 0.50 (normal), 0.25 (busy). Study-guide line: returns 9%, 12%, 15%; expected value 12%, standard deviation 2.12%, coefficient of variation 0.18. Art-supplies line: returns 2%, 14%, 26%; expected value 14%, standard deviation 8.49%, coefficient of variation 0.61. Art supplies has the higher expected return and much more risk per unit of return.
history: Lloyds of London began about 1688 in a coffee house on Tower Street where underwriters shared the risk of ships and cargoes; Harry Markowitz, 1952, first measured investment risk by spread. search: Lloyds coffee house 1688 Edward Lloyd underwriters history
refs: Chapter 11, 'Operating Leverage and Break-even Analysis', section 11.1 'Fixed and Variable Costs' — business risk comes from fixed operating costs, taught there; Chapter 12, 'Financial and Total Leverage', section 12.1 'Borrowing and Fixed Financing Charges' — financial risk comes from borrowing, taught there; Chapter 8, 'Capital Budgeting: Evaluation Techniques', section 8.2 'Net Present Value' — required return is the benchmark against which returns are judged
need: Chapter 8, section 8.2: required return
ladder: familiar case (two routes to work with different delays); reshaped case (two market stalls with different weekly takings); new setting (two product lines with probabilities for slow, normal and busy trade); combined case (three assets compared on average outcome, spread and spread per unit of return)
practice: review: rate of return on a Tk 80,000 bond that pays Tk 5,600 and sells for Tk 82,000; difference between business and financial risk; expected value and standard deviation of a new three-outcome distribution; coefficient of variation of two assets with different expected returns; think: which of two projects carries more risk per unit of return, from new data; pause: after probability distribution; after standard deviation
sources: yes

=== CH10 ===
title: Risk, Required Return and Market Efficiency
purpose: Show how risk sets the return an investor requires, how diversification and beta fit in, and how efficient markets use information.
pace: slow — beta and diversification are new and abstract
style: mixed
words: 5,350
figures: 2
- fig01: the security market line, a rising straight line with two labelled points
- fig02: curve of risk falling as more different assets are added, levelling at a floor
plate: a stock exchange floor with a dense crowd of dealers below a large price board chalked with figures
sections:
10.1 | Risk Premium | framework | 875
10.2 | Diversification and Beta | new idea | 775
10.3 | The Capital Asset Pricing Model | defining theory | 650
10.4 | Efficient Markets | framework | 450
concepts:
- 10.1 | Risk-free rate | minor | needs: Time value of money
- 10.1 | Risk premium | core | needs: Risk, Risk-free rate
- 10.1 | Risk and required return | standard | needs: Required return
- 10.2 | Diversification and portfolio | standard | needs: Risk
- 10.2 | Beta | core | needs: Diversification and portfolio
- 10.3 | Capital asset pricing model | core | needs: Beta, Risk premium
- 10.3 | Security market line | minor | needs: Capital asset pricing model
- 10.4 | Efficient market | standard | needs: Market value
- 10.4 | Weak, semi-strong and strong forms | standard | needs: Efficient market
defines:
- Risk-free rate | the return on an investment with no chance of loss, such as a short government bill
- Risk premium | the extra return investors demand for taking on risk above the risk-free rate
- Portfolio | a collection of investments held together by one investor
- Diversification | spreading money across different investments so that one loss hurts less
- Market risk | risk that affects all investments in a market and cannot be removed by diversifying
- Firm-specific risk | risk that affects one business only and can be reduced by diversifying
- Beta | a number showing how strongly an investment moves with the whole market
- Capital asset pricing model | a formula setting required return as risk-free rate plus beta times the market risk premium
- Security market line | the straight line that shows required return rising with beta
- Efficient market | a market in which prices already reflect the information available
- Weak-form efficiency | prices already reflect all past price information
- Semi-strong-form efficiency | prices reflect all public information, not only past prices
- Strong-form efficiency | prices reflect all information, public and private
assumes: Risk, Return, Rate of return, Expected value, Standard deviation, Required return, Market value, Time value of money
words-in-use: market, premium
case: Meghna and Sons, 20 March 2025. Risk-free rate 8% (a short government bill, assumed for illustration); market return 14%, so market risk premium 6%. Study-guide line: beta 0.6, required return 8% + 0.6 x 6% = 11.6%; expected return 12% (from the previous step), so accept. Art-supplies line: beta 1.5, required return 8% + 1.5 x 6% = 17.0%; expected return 14%, so reject.
history: Black Monday, 19 October 1987: the Dow Jones Industrial Average fell by about a fifth in one day, a test of how markets absorb information. search: Black Monday 19 October 1987 Dow Jones percentage fall
refs: Chapter 9, 'Return and Risk', section 9.2 'What Risk Is' — risk is defined there; Chapter 9, 'Return and Risk', section 9.4 'Measuring the Spread of Outcomes' — standard deviation measures total risk; beta measures market risk; Chapter 8, 'Capital Budgeting: Evaluation Techniques', section 8.2 'Net Present Value' — required return is the project benchmark
need: Chapter 9, sections 9.2 to 9.4; Chapter 8, section 8.2
ladder: clean numbers (risk-free 6%, market 10%, beta 1); realistic numbers (beta 0.6 and 1.5 with a premium of 6%); a decision comparing expected return with required return; a combined problem placing two investments on the security market line
practice: review: required return when the risk-free rate is 7%, market return 13% and beta 1.2; why diversification lowers some risk but not all; draw and read a security market line; three forms of market efficiency with one example of each; think: should an investor accept an asset whose expected return sits below the line — explain; pause: after beta; after the pricing formula
sources: yes

=== CH11 ===
title: Operating Leverage and Break-even Analysis
purpose: Teach cost behaviour, the operating break-even point and the degree of operating leverage.
pace: standard — one worked data set carries the chapter
style: calculation
words: 4,925
figures: 2
- fig01: revenue line and total cost line crossing at the break-even point, with loss and profit areas hatched differently
- fig02: paired bars showing a 12.5% rise in sales and a 50% rise in EBIT
plate: a factory floor with belt-driven machines in a line and a foreman holding a watch near the gate
sections:
11.1 | Fixed and Variable Costs | new idea | 550
11.2 | The Operating Break-even Point | technique | 775
11.3 | Degree of Operating Leverage | technique | 775
11.4 | Operating Leverage and Business Risk | framework | 225
concepts:
- 11.1 | Fixed operating cost | standard | needs: Operating efficiency
- 11.1 | Variable operating cost | standard | needs: Operating efficiency
- 11.1 | Contribution margin | minor | needs: Fixed and variable operating cost
- 11.2 | Operating break-even point | core | needs: Contribution margin
- 11.2 | EBIT at different volumes | standard | needs: EBIT
- 11.3 | Operating leverage | standard | needs: Fixed operating cost
- 11.3 | Degree of operating leverage | core | needs: Operating leverage, EBIT
- 11.4 | Why fixed costs raise business risk | standard | needs: Business risk
defines:
- Fixed operating cost | an operating cost that stays the same whatever the number of units sold
- Variable operating cost | an operating cost that rises and falls with the number of units sold
- Contribution margin | selling price per unit minus variable operating cost per unit
- Operating break-even point | the sales volume at which EBIT is exactly zero
- Operating leverage | the effect of fixed operating costs in making EBIT change by more than sales change
- Degree of operating leverage | the percentage change in EBIT caused by a one percent change in sales
assumes: EBIT, Business risk, Operating efficiency, Profitability, Depreciation
words-in-use: cost, margin, fixed, variable
case: Meghna and Sons, 6 April 2025: the printing unit. Selling price Tk 90 per pack, variable operating cost Tk 50, contribution margin Tk 40, fixed operating cost Tk 240,000 a year. Break-even 240,000 / 40 = 6,000 packs. EBIT: 7,000 packs Tk 40,000; 8,000 packs Tk 80,000; 9,000 packs Tk 120,000. Degree of operating leverage at 8,000 packs = 320,000 / 80,000 = 4.00; at 9,000 packs = 360,000 / 120,000 = 3.00. A 12.5% rise in sales from 8,000 to 9,000 packs lifts EBIT by 50%.
history: Henry Ford, Highland Park, Michigan, from 1913: the moving assembly line raised fixed costs and cut variable cost, so the Model T price fell from 850 dollars in 1908 to about 260 in 1925. search: Ford Model T price 1908 1925 moving assembly line Highland Park
refs: Chapter 9, 'Return and Risk', section 9.2 'What Risk Is' — business risk was defined there; Chapter 7, 'Capital Budgeting: Decisions and Cash Flows', section 7.3 'Cash Flow from an Investment' — EBIT was defined there
need: Chapter 9, section 9.2: business risk; Chapter 7, section 7.3: EBIT
ladder: clean numbers (price Tk 20, variable cost Tk 12, fixed cost Tk 8,000); realistic numbers (price Tk 90, variable cost Tk 50, fixed cost Tk 240,000); the degree of operating leverage at two volumes; a combined problem that finds the break-even point and the leverage at a chosen volume
practice: review: break-even point for price Tk 150, variable cost Tk 95 and fixed cost Tk 330,000; EBIT at three volumes around break-even; degree of operating leverage at one chosen volume; why leverage is higher near break-even; think: a firm swaps variable cost for fixed cost — compute the effect on break-even and leverage; pause: after the break-even formula
sources: no

=== CH12 ===
title: Financial and Total Leverage
purpose: Teach how borrowing and preferred stock magnify earnings per share, and how operating and financial leverage combine.
pace: standard — extends one data set from Chapter 11
style: calculation
words: 5,025
figures: 1
- fig01: three connected bars for sales, EBIT and earnings per share showing growing percentage changes
plate: a lever and fulcrum in a workshop with a small hand pressing down to lift a heavy cast weight
sections:
12.1 | Borrowing and Fixed Financing Charges | new idea | 875
12.2 | Degree of Financial Leverage | technique | 775
12.3 | Degree of Total Leverage | technique | 550
12.4 | Leverage and the Risk of a Firm | framework | 225
concepts:
- 12.1 | Financial leverage | core | needs: Debt, Financial risk
- 12.1 | Fixed financing charges | minor | needs: Debt
- 12.1 | Common stock and preferred stock | standard | needs: Equity, Dividend
- 12.2 | Earnings per share | standard | needs: Common stock
- 12.2 | Degree of financial leverage | core | needs: Earnings per share, EBIT
- 12.3 | Degree of total leverage | core | needs: Degree of operating leverage, Degree of financial leverage
- 12.4 | Leverage and total risk | standard | needs: Business risk, Financial risk
defines:
- Financial leverage | the effect of fixed financing charges in making earnings per share change by more than EBIT
- Fixed financing charge | interest or preferred dividend that must be paid whatever the level of EBIT
- Common stock | ownership shares that receive what remains after all other claims, with no fixed dividend
- Preferred stock | ownership shares with a fixed dividend that is paid before any dividend on common stock
- Earnings per share | profit left for common stock owners divided by the number of common shares
- Degree of financial leverage | the percentage change in earnings per share caused by a one percent change in EBIT
- Degree of total leverage | the percentage change in earnings per share caused by a one percent change in sales
assumes: EBIT, Operating leverage, Degree of operating leverage, Business risk, Financial risk, Debt, Equity, Dividend, Fixed operating cost, Contribution margin
words-in-use: share, charge
case: Meghna and Sons Limited, 1 July 2025 (company registered to own the shop and printing unit; the 8,000-pack data of Chapter 11 is repeated). Sales 8,000 packs at Tk 90, variable cost Tk 50, fixed operating cost Tk 240,000, so EBIT Tk 80,000. Printing unit financing: bank loan Tk 200,000 at 10% (interest Tk 20,000); Tk 100,000 of preferred stock at 12% (dividend Tk 12,000); 5,000 common shares; tax 25%. Earnings before tax Tk 60,000; tax Tk 15,000; profit after tax Tk 45,000; less preferred dividend Tk 12,000 leaves Tk 33,000; earnings per share Tk 6.60. Degree of operating leverage 4.00; degree of financial leverage 80,000 / (80,000 - 20,000 - 12,000/0.75) = 80,000 / 44,000 = 1.82; degree of total leverage 4.00 x 1.8182 = 7.27. At 9,000 packs EBIT is Tk 120,000, profit after tax Tk 75,000, ordinary earnings Tk 63,000, earnings per share Tk 12.60 (up 90.9% on a 12.5% rise in sales).
history: RJR Nabisco, 1988: the buyout by Kohlberg Kravis Roberts, worth about 25 billion dollars and funded largely by debt, showed how borrowing magnifies gains and losses. search: RJR Nabisco 1988 leveraged buyout KKR 25 billion
refs: Chapter 11, 'Operating Leverage and Break-even Analysis', section 11.3 'Degree of Operating Leverage' — operating leverage is the first link in total leverage; Chapter 9, 'Return and Risk', section 9.2 'What Risk Is' — financial risk was defined there; Chapter 14, 'Valuing Common and Preferred Stock', section 14.1 'Rights of Shareholders' — rights of common and preferred shareholders are taught fully there
need: Chapter 11, sections 11.2 and 11.3; Chapter 9, section 9.2
ladder: clean numbers (EBIT Tk 10,000, interest Tk 2,000, 1,000 shares); realistic numbers (the Chapter 11 data set with interest, preferred dividend, tax and 5,000 shares); a combined problem finding all three degrees; a reshaped problem where the firm replaces debt with equity
practice: review: earnings per share from new EBIT, interest, preferred dividend, tax rate and share count; degree of financial leverage; degree of total leverage as the product of two degrees; effect of a 10% rise in sales on earnings per share; think: compare two financing plans for the same EBIT and say which is riskier; pause: after the financial leverage formula
sources: no

=== CH13 ===
title: Valuing Bonds
purpose: Define a bond, explain value, and compute bond values and yield to maturity.
pace: slow — present value of two kinds of payment at once
style: calculation
words: 5,250
figures: 2
- fig01: timeline of a five-year bond, five coupon arrows and one larger arrow at maturity
- fig02: bond value falling as the required return rises, with a horizontal line at par value
plate: an engravers workbench with a copper plate, a burin and a half-cut border for a certificate
sections:
13.1 | What a Bond Is | new idea | 775
13.2 | The Idea of Value | framework | 325
13.3 | The Value of a Bond | technique | 775
13.4 | Discount, Par and Premium Bonds | framework | 775
concepts:
- 13.1 | Bond | core | needs: Debt
- 13.1 | Par value, coupon interest and maturity | standard | needs: Bond
- 13.2 | Valuation and intrinsic value | standard | needs: Present value
- 13.2 | Intrinsic and market value compared | minor | needs: Market value
- 13.3 | Value of a bond with yearly interest | core | needs: Present value of an ordinary annuity, Present value
- 13.3 | Bonds paying interest twice a year | standard | needs: Compounding period
- 13.4 | Discount, par and premium bonds | core | needs: Value of a bond with yearly interest, Required return
- 13.4 | Yield to maturity | standard | needs: Internal rate of return
defines:
- Valuation | the work of finding what an asset is worth from the cash it is expected to bring
- Intrinsic value | the worth of an asset worked out from the present value of its expected cash flows
- Bond | a loan sold to investors, repaid at a set date with fixed interest along the way
- Par value | the amount printed on a bond that is repaid at maturity
- Coupon interest | the fixed interest a bond pays each period, set as a percentage of par value
- Maturity | the date on which a bond is repaid
- Discount bond | a bond that sells for less than its par value
- Premium bond | a bond that sells for more than its par value
- Yield to maturity | the yearly return an investor earns by buying a bond at its price and holding it to maturity
assumes: Present value, Discount rate, Required return, Ordinary annuity, Present value of an ordinary annuity, Internal rate of return, Market value, Compounding period, Debt
words-in-use: par, yield, face
case: Meghna and Sons Limited, 3 August 2025. A corporate bond: par value Tk 1,000, coupon 9% a year (Tk 90), 5 years to maturity. Required return 12%: value Tk 891.86; required return 9%: Tk 1,000.00; required return 7%: Tk 1,082.00. Half-yearly interest (coupon Tk 45, 10 periods) at 12% a year (6% per half-year): Tk 889.60. A bond priced at Tk 920 with the same coupon and 5 years left has yield to maturity about 11.2%.
history: British Consols, 1751: Henry Pelham merged several state debts into one bond paying 3 per cent with no repayment date, creating a long-lived market in bonds. search: Consols 1751 Henry Pelham consolidation national debt 3 per cent
refs: Chapter 5, 'Annuities', section 5.3 'Present Value of an Annuity' — bond interest is an annuity; Chapter 4, 'The Time Value of Money: Single Sums', section 4.3 'Present Value of a Single Sum' — par value is a single sum discounted; Chapter 8, 'Capital Budgeting: Evaluation Techniques', section 8.3 'Internal Rate of Return and Profitability Index' — yield to maturity uses the same idea as internal rate of return; Chapter 14, 'Valuing Common and Preferred Stock', section 14.4 'Comparing Bonds and Stock' — bonds and stock are compared after both are valued
need: Chapter 5, section 5.3; Chapter 4, section 4.3; Chapter 8, sections 8.2 and 8.3
ladder: clean numbers (par Tk 1,000, coupon 10%, 3 years, required return 10%); realistic numbers (coupon 9%, 5 years, required return 12%); a premium bond and a discount bond from the same coupon; a combined problem with half-yearly interest and a yield to maturity found by trial
practice: review: value of a 4-year bond with par Tk 1,000, coupon 8% and required return 10%; the relation between required return and coupon rate for discount, par and premium; half-yearly interest on a new bond; yield to maturity found between two trial rates; think: a bond sells for Tk 940 — say whether the market requires more or less than the coupon rate and show the check; pause: after the bond value formula
sources: no

=== CH14 ===
title: Valuing Common and Preferred Stock
purpose: Explain what stock owners hold, value preferred and common stock, and compare stock with bonds.
pace: standard — reuses perpetuity and discounting
style: mixed
words: 4,600
figures: 1
- fig01: timeline of yearly dividends growing at a steady percentage, bars rising from left to right
plate: a ship at an Amsterdam quay with merchants inspecting cargo chests under a warehouse crane
sections:
14.1 | Rights of Shareholders | framework | 550
14.2 | Valuing Preferred Stock and Level Dividends | technique | 450
14.3 | Valuing Stock with Growing Dividends | technique | 775
14.4 | Comparing Bonds and Stock | framework | 225
concepts:
- 14.1 | Voting right and residual claim | standard | needs: Common stock
- 14.1 | Capital gain | minor | needs: Return
- 14.1 | Preferred stock features | standard | needs: Preferred stock
- 14.2 | Value of preferred stock | standard | needs: Perpetuity
- 14.2 | Value of stock with level dividends | standard | needs: Perpetuity
- 14.3 | Dividend growth model | core | needs: Present value, Required return
- 14.3 | Required return on stock | standard | needs: Capital asset pricing model
- 14.4 | Bond and stock compared | standard | needs: Bond, Common stock
defines:
- Voting right | the right of a shareholder to vote on matters such as choosing directors
- Residual claim | the right to what is left of a business income and assets after all others are paid
- Capital gain | the profit made when an asset is sold for more than it cost
- Dividend growth model | a formula valuing stock as next dividend divided by required return minus growth rate
assumes: Common stock, Preferred stock, Dividend, Perpetuity, Present value, Required return, Intrinsic value, Bond, Risk, Capital asset pricing model
words-in-use: share, stock
case: Meghna and Sons Limited, 7 September 2025. Preferred stock paying a fixed Tk 8 a year, required return 10%: value Tk 80.00. Common stock with a level dividend of Tk 5 and required return 10%: value Tk 50.00. Common stock of a publishing firm: last dividend Tk 4.00, growth 6% a year for ever, required return 12%: next dividend Tk 4.24; value 4.24 / (0.12 - 0.06) = Tk 70.67.
history: Dutch East India Company, 1602: shares in the company were traded in Amsterdam, and many historians treat it as the first widely traded joint-stock company. search: Dutch East India Company 1602 shares Amsterdam first stock exchange
refs: Chapter 12, 'Financial and Total Leverage', section 12.1 'Borrowing and Fixed Financing Charges' — common and preferred stock were first defined there; Chapter 5, 'Annuities', section 5.1 'What an Annuity Is' — a fixed dividend forever is a perpetuity; Chapter 13, 'Valuing Bonds', section 13.4 'Discount, Par and Premium Bonds' — bond value is worked in the same way
need: Chapter 12, section 12.1; Chapter 5, section 5.1; Chapter 13, section 13.2
ladder: clean numbers (dividend Tk 6 forever, required return 12%); realistic numbers (last dividend Tk 4, growth 6%, required return 12%); two stocks compared at different growth rates; a combined problem valuing a bond and a stock for the same investor
practice: review: value of preferred stock paying Tk 9 a year at required return 12%; value of common stock with last dividend Tk 3.50, growth 5% and required return 11%; why the growth rate must stay below the required return; three differences between a bond and a stock; think: choose between a bond and a stock for a cautious investor and justify; pause: after the growth formula
sources: yes

=== CH15 ===
title: Working Capital and the Cash Cycle
purpose: Define working capital, trace the cash conversion cycle, explain float, and estimate working capital needs.
pace: slow — the cycle has several timed parts that must be added and subtracted
style: mixed
words: 5,250
figures: 2
- fig01: horizontal timeline showing inventory days, receivable days and payable days, and the gap that is the cash conversion cycle
- fig02: timeline of a cheque with three labelled stages, mail, processing and clearing
plate: a warehouse with sacks and barrels, a clerk counting stock with a tally board and a loaded cart at the door
sections:
15.1 | What Working Capital Is | new idea | 1000
15.2 | The Cash Conversion Cycle | technique | 775
15.3 | Float | technique | 325
15.4 | Estimating Working Capital Needs | technique | 550
concepts:
- 15.1 | Working capital and net working capital | core | needs: Asset, Liability
- 15.1 | Current assets and current liabilities | standard | needs: Asset, Liability
- 15.1 | Inventory, accounts receivable and accounts payable | standard | needs: Current assets
- 15.2 | Operating cycle | standard | needs: Inventory
- 15.2 | Cash conversion cycle | core | needs: Operating cycle, Accounts payable
- 15.3 | Float | standard | needs: Cash flow
- 15.3 | Mail, processing and clearing float | minor | needs: Float
- 15.4 | Estimating the requirement | core | needs: Cash conversion cycle
defines:
- Working capital | the money a business has tied up in short-term assets such as stock and money owed by customers
- Net working capital | current assets minus current liabilities
- Current asset | an asset expected to turn into cash within about a year
- Current liability | an amount owed that must be paid within about a year
- Inventory | goods held for sale or for use in making goods
- Accounts receivable | money owed to a business by customers who bought on credit
- Accounts payable | money a business owes to suppliers for goods bought on credit
- Operating cycle | the days from buying stock to collecting cash from the customer
- Cash conversion cycle | the operating cycle minus the days a business takes to pay its suppliers
- Float | money that has been paid by one person but cannot yet be used by the receiver
assumes: Asset, Liability, Cash flow, Liquidity, Use of funds, Source of funds, Funds
words-in-use: account, current, stock
case: Meghna and Sons Limited, 1 September 2025 (figures for the 12 months to 31 August 2025). Sales Tk 9,000,000; cost of sales Tk 6,300,000. Inventory held 75 days, customers (school accounts) pay in 20 days, suppliers are paid in 45 days. Operating cycle 95 days; cash conversion cycle 50 days. Using a 365-day year: inventory Tk 1,294,521, receivables (on sales) Tk 493,151, payables Tk 776,712; net investment in the cycle Tk 1,010,959. Float: on 3 February 2025 a customer posted a cheque for Tk 200,000; mail 2 days, processing 1 day, clearing 3 days; total float 6 days; money unused at 10% a year costs 200,000 x 0.10 x 6/365 = Tk 329.
history: W. T. Grant, United States, 1975: a large retailer reporting profits went bankrupt because cash tied up in credit sales and stock ran out. search: W. T. Grant bankruptcy 1975 cash flow Largay Stickney
refs: Chapter 2, 'The Financial Manager's Work', section 2.3 'How Finance Shapes Business Success' — liquidity was introduced there; Chapter 1, 'Money, Finance and the Firm', section 1.3 'Where Funds Come From and Where They Go' — assets and liabilities were defined there; Chapter 16, 'Short-Term Financing and Matching', section 16.4 'The Matching Principle' — financing the working capital need is taught there
need: Chapter 2, section 2.3; Chapter 1, section 1.3
ladder: clean numbers (inventory 30 days, receivables 10 days, payables 20 days); realistic numbers (75, 20 and 45 days on annual sales of Tk 9,000,000); a reshaped case where the supplier shortens payment time; a combined problem estimating the cash needed and the cost of float
practice: review: the cash conversion cycle for a firm with inventory 60 days, receivables 25 days and payables 35 days; the cash invested in that cycle from new sales data; the three parts of float for a cheque and the days lost; why a faster collection shortens the cycle; think: estimate working capital for a shop that expects to grow sales by one fifth; pause: after the operating cycle; after the cash conversion cycle
sources: no

=== CH16 ===
title: Short-Term Financing and Matching
purpose: Classify financing by time span, cost trade credit, survey other short-term sources and apply the matching principle.
pace: standard — builds on Chapter 15
style: mixed
words: 4,800
figures: 1
- fig01: stepped area over twelve months showing permanent and seasonal needs, with bands labelled long-term finance and short-term finance
plate: a bank counter hall with a queue of anxious depositors facing a clerk behind a wire grille
sections:
16.1 | Financing by Time Span | framework | 325
16.2 | Trade Credit | technique | 875
16.3 | Other Short-Term Sources | framework | 225
16.4 | The Matching Principle | framework | 775
concepts:
- 16.1 | Short-term, intermediate-term and long-term financing | standard | needs: Source of funds
- 16.1 | Where each span is usually found | minor | needs: Debt, Equity
- 16.2 | Trade credit and credit terms | standard | needs: Accounts payable
- 16.2 | Cash discount | minor | needs: Trade credit
- 16.2 | The cost of giving up a cash discount | core | needs: Interest rate, Cash discount
- 16.3 | Bank overdraft and short loans | standard | needs: Debt
- 16.4 | Matching principle | core | needs: Current assets
- 16.4 | Permanent and seasonal working capital | standard | needs: Working capital
defines:
- Short-term financing | money borrowed or raised for up to one year
- Intermediate-term financing | money borrowed or raised for between one and five years
- Long-term financing | money borrowed or raised for more than five years
- Trade credit | a supplier allowing a business to pay for goods some days after delivery
- Credit terms | the discount offered and the days allowed for payment, written like 2/10, net 40
- Cash discount | a reduction in price for paying a supplier early
- Bank overdraft | a bank allowing an account to go below zero up to an agreed limit, with interest charged on the amount used
- Matching principle | the rule that short-term needs are financed with short-term funds and long-term needs with long-term funds
- Permanent working capital | the minimum working capital a business needs all year
- Seasonal working capital | extra working capital needed only at the busiest times of the year
assumes: Working capital, Net working capital, Accounts payable, Current asset, Current liability, Cash conversion cycle, Interest rate, Debt, Equity, Source of funds
words-in-use: term, cash
case: Meghna and Sons Limited, 1 October 2025. A supplier invoice of Tk 500,000 on terms 2/10, net 40. Taking the discount saves Tk 10,000 and costs Tk 490,000 on day 10. Cost of giving up the discount = 2/98 x 365/30 = 24.83% a year (approximation). A bank overdraft costs 14%: borrowing Tk 490,000 for 30 days costs Tk 5,638, which is less than the Tk 10,000 discount, so take the discount and borrow. Seasonal need: permanent current assets Tk 900,000 financed long term; a seasonal peak of Tk 400,000 for 3 months before the January term. Short-term borrowing at 14% costs Tk 14,000; carrying the Tk 400,000 on a long-term loan at 11% for the whole year costs Tk 44,000.
history: Northern Rock, United Kingdom, 2007: the bank funded long-term mortgages with short-term borrowing; when lenders stopped, depositors queued on 14 September 2007. search: Northern Rock run 14 September 2007 short-term funding mortgages
refs: Chapter 15, 'Working Capital and the Cash Cycle', section 15.1 'What Working Capital Is' — working capital is defined there; Chapter 15, 'Working Capital and the Cash Cycle', section 15.2 'The Cash Conversion Cycle' — the cycle sets the size of the need; Chapter 17, 'Long-Term Financing and the Cost of Capital', section 17.1 'Term Loans and Debt Instruments' — long-term debt instruments are taught there
need: Chapter 15, sections 15.1 and 15.2
ladder: clean numbers (invoice Tk 10,000, terms 2/10 net 30); realistic numbers (invoice Tk 500,000, terms 2/10 net 40, bank rate 14%); two different credit terms compared; a combined problem matching a seasonal need with the cheapest source
practice: review: cost of giving up the discount on terms 3/10 net 45; whether to borrow from the bank at 13% to take the discount; define short, intermediate and long span with a source for each; apply the matching principle to a toy shop with a December peak; think: choose how to fund a permanent need and a seasonal need and compute the interest saved; pause: after the cost of giving up a discount
sources: no

=== CH17 ===
title: Long-Term Financing and the Cost of Capital
purpose: Survey the sources of long-term funds, the capital market and Bangladeshi institutions, and compute the cost of capital.
pace: standard — broad survey followed by one calculation
style: mixed
words: 5,600
figures: 1
- fig01: horizontal bar split into debt, preferred stock and common equity by hatching, each labelled with its weight
plate: a shipyard with a half-built iron steamship on the slips, scaffolding and workmen at the hull
sections:
17.1 | Term Loans and Debt Instruments | framework | 775
17.2 | Owners' Funds as Long-Term Finance | framework | 450
17.3 | Raising Funds in the Capital Market | framework | 450
17.4 | Long-Term Lenders and Investors in Bangladesh | framework | 550
17.5 | The Cost of Capital | technique | 775
concepts:
- 17.1 | Term loan and its conditions | core | needs: Debt, Interest rate
- 17.1 | Debentures and mortgages | standard | needs: Bond, Debt
- 17.2 | Retained earnings as a source | standard | needs: Retained earnings
- 17.2 | Issuing common and preferred stock | standard | needs: Common stock, Preferred stock
- 17.3 | Capital market | standard | needs: Financial market
- 17.3 | Public issue and rights issue | standard | needs: Common stock
- 17.4 | Institutions that supply long-term funds | core | needs: Term loan, Capital market
- 17.5 | Cost of capital | core | needs: Required return, Interest rate
- 17.5 | Weighted average cost of capital | standard | needs: Cost of capital
defines:
- Term loan | a loan repaid in instalments over a fixed period of more than one year
- Collateral | an asset a borrower pledges so the lender can take it if the loan is not repaid
- Covenant | a promise in a loan agreement that limits what the borrower may do
- Debenture | a bond backed only by the general standing of the borrower, with no particular asset pledged
- Capital market | the market where long-term loans and shares are issued and traded
- Initial public offering | the first sale of a business shares to the general public
- Rights issue | an offer of new shares to existing shareholders in proportion to what they already hold
- Cost of capital | the average return a business must earn on its funds to satisfy those who supplied them
- Weighted average cost of capital | the cost of each source of funds multiplied by its share of the total and added together
assumes: Term loan basics, Debt, Equity, Bond, Common stock, Preferred stock, Retained earnings, Financial market, Required return, Interest rate, Long-term financing, Net present value
words-in-use: capital, term
case: Meghna and Sons Limited, 15 November 2025. Long-term funds: bank loans outstanding Tk 500,000 at 10% (after tax 10% x 0.75 = 7.5%); preferred stock Tk 100,000 at 12%; common equity including retained earnings Tk 2,400,000 with a required return of 14%. Total Tk 3,000,000; weights 16.67%, 3.33% and 80.00%. Weighted average cost of capital = 0.1667 x 7.5% + 0.0333 x 12% + 0.80 x 14% = 1.25% + 0.40% + 11.20% = 12.85%. A new term loan of Tk 600,000 over 5 years at 10% needs yearly instalments of Tk 158,278.49; collateral: the machine; covenant: owners equity must stay above Tk 2,000,000. Project Press (Chapter 8 flows) at 12.85%: net present value Tk 172,667, still positive.
history: Ford Motor Company, 1956: the largest public share offering of its time, in which the Ford Foundation sold shares to the public. search: Ford Motor Company 1956 initial public offering largest Ford Foundation. For Bangladesh, verify current names: Bangladesh Development Bank Limited, Investment Corporation of Bangladesh, IDCOL, commercial banks, Dhaka Stock Exchange, Chittagong Stock Exchange, Bangladesh Securities and Exchange Commission
refs: Chapter 16, 'Short-Term Financing and Matching', section 16.1 'Financing by Time Span' — time spans of financing were set out there; Chapter 13, 'Valuing Bonds', section 13.1 'What a Bond Is' — bond features were set out there; Chapter 14, 'Valuing Common and Preferred Stock', section 14.1 'Rights of Shareholders' — rights of shareholders were set out there; Chapter 8, 'Capital Budgeting: Evaluation Techniques', section 8.2 'Net Present Value' — cost of capital is the discount rate in net present value
need: Chapter 16, section 16.1; Chapter 13, section 13.1; Chapter 14, section 14.1
ladder: clean numbers (debt Tk 400,000 at 8% after tax and equity Tk 600,000 at 12%); realistic numbers (the case file with three sources of funds); a reshaped case where the debt share rises; a combined problem using the cost of capital as the discount rate for a project
practice: review: name three conditions a lender may attach to a term loan and say why; compare a rights issue with a public issue; list four types of lender or market venue for long-term funds in Bangladesh and what each supplies; after-tax cost of a loan at 11% with a 25% tax rate; weighted cost from new weights; think: a new firm asks why long-term funds suit its first machines and shop fit-out — explain in plain words; pause: after the debenture and mortgage section; after weighting
sources: no

=== CH18 ===
title: Leasing
purpose: Define a lease, separate the main kinds, and compare leasing with buying by present value of after-tax cash outflows.
pace: standard — final chapter uses earlier tools together
style: mixed
words: 4,575
figures: 1
- fig01: two timelines of yearly after-tax outflows, one for leasing and one for buying, aligned by year
plate: a carriage-hire yard with a hansom cab, an ostler holding the horse and the owner and hirer shaking hands
sections:
18.1 | What a Lease Is | new idea | 650
18.2 | Kinds of Lease | framework | 550
18.3 | The Lease or Buy Decision | technique | 775
concepts:
- 18.1 | Lease | core | needs: Asset
- 18.1 | Lessor and lessee | minor | needs: Lease
- 18.2 | Operating lease | standard | needs: Lease
- 18.2 | Financial lease | standard | needs: Lease
- 18.2 | Sale and leaseback | minor | needs: Financial lease
- 18.3 | The lease-or-buy decision | core | needs: Net present value, Present value
- 18.3 | After-tax cash outflow | standard | needs: Depreciation, Interest rate
defines:
- Lease | a contract in which the owner of an asset lets another party use it in return for regular payments
- Lessor | the owner who lets an asset out under a lease
- Lessee | the party who pays to use an asset under a lease
- Operating lease | a short or cancellable lease where the lessor keeps most of the risks of owning the asset
- Financial lease | a long lease that cannot be cancelled, in which the lessee in effect carries the cost and risk of owning
- Sale and leaseback | selling an asset to a lessor and at once leasing it back
- After-tax cash outflow | cash paid out in a year after taking away the tax saved by that payment
assumes: Asset, Depreciation, Present value, Annuity, Ordinary annuity, Cost of capital, Net present value, Interest rate, Amortisation, Term loan
words-in-use: lease, rent
case: Meghna and Sons Limited, 3 December 2025. A second machine, cost Tk 780,000. Lease: Tk 210,000 at each year end for 5 years; after tax at 25% Tk 157,500 a year. Buy: loan Tk 780,000 at 10% repaid in 5 equal year-end instalments of Tk 205,762.04; depreciation Tk 156,000 a year; no resale value assumed. Yearly after-tax cost of buying (instalment less 25% of interest plus depreciation): year 1 Tk 147,262; year 2 Tk 150,456; year 3 Tk 153,970; year 4 Tk 157,834; year 5 Tk 162,086. Discount rate: after-tax cost of debt 7.5%. Present value of leasing Tk 637,227; present value of buying Tk 622,210; the lower cost is buying.
history: Early modern equipment leasing in the United States, early 1950s: a firm formed in San Francisco in 1952 is usually cited as the first to lease equipment as a business; verify name and date. search: first equipment leasing company 1952 San Francisco history leasing industry
refs: Chapter 17, 'Long-Term Financing and the Cost of Capital', section 17.5 'The Cost of Capital' — after-tax cost of debt is the discount rate for the comparison; Chapter 8, 'Capital Budgeting: Evaluation Techniques', section 8.2 'Net Present Value' — net present value method is reused; Chapter 5, 'Annuities', section 5.4 'Finding the Payment' — loan payments follow the amortisation method
need: Chapter 17, section 17.5; Chapter 8, section 8.2; Chapter 5, section 5.4
ladder: familiar case (renting or buying a bicycle for a delivery round); reshaped case (a machine with a loan and tax); new setting (a clinic leasing a scanner); combined case (a lease against a loan, with tax shield and present value at the after-tax rate)
practice: review: difference between an operating lease and a financial lease with an example of each; the four steps of the lease-or-buy decision applied to new data; after-tax outflow of a lease payment at a 25% tax rate; present value of a lease against a loan from new data; think: the lessor offers a lower payment but a shorter term — decide using present values; pause: after the after-tax outflow idea
sources: no

=== A1 ===
title: Maths You Will Use
purpose: A one-to-two-page reference for the arithmetic used in the book; not a lesson.
pace: standard — reference only
style: calculation
words: 1,000
figures: 0
sections:
A1.1 | Percentages and percentage change | reference | 170
A1.2 | Ratios | reference | 150
A1.3 | Averages and weighted averages | reference | 170
A1.4 | Rearranging an equation | reference | 170
A1.5 | Powers, roots and rounding | reference | 170
A1.6 | Reading a graph | reference | 170
concepts:
- A1 | each topic is a plain definition and one tiny worked example using taka | minor | needs: none
defines: none
assumes: none
words-in-use: none
case: none
history: none
refs: Chapter 4, 'The Time Value of Money: Single Sums', section 4.2 'Future Value of a Single Sum' — powers are used there; Chapter 9, 'Return and Risk', section 9.3 'Describing Uncertain Outcomes' — weighted averages are used there
need: nothing
ladder: not applicable
practice: none
sources: no
