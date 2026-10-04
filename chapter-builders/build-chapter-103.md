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
course: MGT-103 Principles of Accounting
book: Principles of Accounting
spelling: British · dates: day, month, year (12 March 2026) · country: mixed, no single country; the running case is set in Bangladesh, examples and history draw on several countries, and the place is named whenever law or standards differ · currency and numbers: taka for the running case, written Tk 158,000 (international digit grouping); a historical case keeps its own currency
running case: Meghna and Sons — illustrative documented case file. Base facts (FIXED): Meghna and Sons, a family-run bookshop in Dhanmondi, Dhaka, owner Nurul Meghna, trading since 4 April 1996. 2025 results: net sales Tk 14,400,000; cost of goods sold Tk 9,800,000; gross profit Tk 4,600,000; operating expenses Tk 3,850,000; profit Tk 750,000. A separate book-binding workshop, Meghna Binding Service (illustrative separate set of books), began on 1 January 2026. Each chapter's case slice states its own figures.
chosen words (one term, one word):
- use revenue — also called income, sales income (income statement is a fixed title)
- use owner's equity — also called capital, net worth, proprietorship
- use account payable — also called creditor
- use account receivable — also called debtor
- use inventory — also called stock of goods, merchandise on hand (use stock only for company shares, never for goods)
- use statement of financial position — also called balance sheet (use balance sheet)
size plan: 20 chapters plus appendix A1, ~104775 words, ~400 pages (target 400)
chapters:
1. Business Activity and Companies
2. What Accounting Is
3. The Accounting Equation
4. Accounts, Debits and Credits
5. The Journal and the Ledger
6. The Trial Balance
7. Adjusting the Accounts
8. The Worksheet
9. Financial Statements and Closing Entries
10. Accounts of a Merchandising Company
11. Special Journals
12. The Cash Book and Bank Reconciliation
13. Inventory: Meaning and Measurement
14. Costing Inventory: FIFO, LIFO and Weighted Average
15. Plant Assets
16. Depreciation
17. Accounts of Clubs and Sole Traders
18. Partnership Accounts
19. Company Accounts: Shares
20. The Whole Cycle and Reporting Periods
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
title: Business Activity and Companies
purpose: Shows the kinds of business activity and the forms of business that need accounting records.
pace: slow — first chapter; teaches how to read the book and the words of business
style: descriptive
words: 5175
figures: 1
- fig01: service, merchandising and manufacturing businesses as three chains
plate: a Victorian counting-house clerk at a high desk among ledgers, 1880s
sections:
1.1 | What Businesses Do | new idea | 1225
1.2 | Who Owns the Business | framework | 900
1.3 | Why Records Are Needed | framework | 450
concepts:
- 1.1 | business activity | core | needs: none
- 1.1 | service business | standard | needs: business activity
- 1.1 | merchandising business | standard | needs: service business
- 1.1 | manufacturing business | standard | needs: merchandising business
- 1.2 | sole proprietor | standard | needs: manufacturing business
- 1.2 | partnership | standard | needs: sole proprietor
- 1.2 | company | standard | needs: partnership
- 1.2 | shareholder | standard | needs: company
- 1.3 | need for records | standard | needs: shareholder
- 1.3 | decisions that depend on records | standard | needs: need for records
defines:
- Merchandising business | A business that buys finished goods and sells them again at a higher price, such as a bookshop.
- Service business | A business that earns money by doing work for customers rather than by selling goods.
- Manufacturing business | A business that makes goods from materials and then sells them.
- Company | A business that is a legal person separate from its owners, whose capital is split into shares.
- Shareholder | A person who owns one or more shares of a company.
assumes: none
words-in-use: business; record
case: Base facts: the shop is a merchandising business (buys books and sells them); Meghna Binding Service begins 1 January 2026 as a service business (repairs and binds books for schools); 2025 net sales Tk 14,400,000; owner Nurul Meghna; a proposal exists to form a company in 2026 with 410,000 shares of Tk 10 (Tk 4,100,000).
history: English East India Company, 31 December 1600: received a royal charter and sold shares to merchants to fund voyages, an early company with records for its investors | search: East India Company charter 31 December 1600
refs: none
need: nothing
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: classify three firms by activity; list who needs records of a bakery; think: why a service business keeps no goods records; pause: after merchandising business
sources: no

=== CH02 ===
title: What Accounting Is
purpose: Defines accounting, its branches, its users and the rules that make its numbers trustworthy.
pace: slow — second chapter; teaches the purpose of the whole book
style: descriptive
words: 5075
figures: 1
- fig01: accounting as a flow from events to reports to users
plate: a Victorian auditors at a long table examining ledgers by lamplight
sections:
2.1 | The Idea of Accounting | new idea | 675
2.2 | Who Uses Accounting Information | framework | 675
2.3 | Rules and Concepts | framework | 1125
concepts:
- 2.1 | accounting | standard | needs: none
- 2.1 | bookkeeping | standard | needs: accounting
- 2.1 | branches of accounting | standard | needs: bookkeeping
- 2.2 | internal users | standard | needs: branches of accounting
- 2.2 | external users | standard | needs: internal users
- 2.2 | what each user asks | standard | needs: external users
- 2.3 | accounting standards | standard | needs: what each user asks
- 2.3 | business entity | standard | needs: accounting standards
- 2.3 | going concern | standard | needs: business entity
- 2.3 | historical cost | standard | needs: going concern
- 2.3 | financial statements | standard | needs: historical cost
defines:
- Accounting | The process of recording, sorting and reporting the money effects of a business's events to people who need them.
- Bookkeeping | The recording part of accounting: writing down each transaction in the right book.
- Financial accounting | Accounting that prepares reports for outsiders, such as lenders, owners and tax authorities.
- Management accounting | Accounting that prepares reports inside the business to help managers plan and control.
- Accounting standard | A written rule about how to record and report items, so that different businesses report in the same way.
- Business entity | The rule that a business is treated as separate from its owner when records are kept.
- Going concern | The assumption that a business will continue to operate for the foreseeable future.
- Historical cost | The rule of recording an asset at the price paid for it, not at its present value.
- Financial statement | A formal report of a business's results or position, prepared at the end of a period.
assumes: Merchandising business, Service business, Manufacturing business
words-in-use: account; standard
case: Users of the shop's 2025 records: Nurul (profit Tk 750,000); the bank (loan Tk 600,000); publishers (owed Tk 1,150,000); the tax authority (profit return); staff (wages Tk 2,940,000).
history: International Accounting Standards Committee, 29 June 1973: formed in London by professional bodies from several countries to write common standards | search: IASC founded 1973 London
refs: Chapter 1, 'Business Activity and Companies', section 1.1 'What Businesses Do' — business activity; Chapter 1, 'Business Activity and Companies', section 1.3 'Why Records Are Needed' — why records are needed
need: you-will-need: Chapter 1 'Business Activity and Companies', section 1.1 'What Businesses Do'; Chapter 1 'Business Activity and Companies', section 1.3 'Why Records Are Needed'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: match users to questions; think: why a shop's and a home's money must be kept apart; pause: after business entity
sources: no

=== CH03 ===
title: The Accounting Equation
purpose: Teaches assets, liabilities and owner's equity, and how every transaction keeps the equation in balance.
pace: slow — the core idea of the book; new and abstract
style: calculation
words: 5075
figures: 2
- fig01: the equation as a balanced scale
- fig02: effect of seven transactions on the equation as a table
plate: a Victorian grocer's balance scale with weights on both pans and a ledger beside it
sections:
3.1 | Assets, Liabilities and Equity | new idea | 900
3.2 | Revenue, Expenses and Drawings | framework | 900
3.3 | Transactions and the Equation | technique | 675
concepts:
- 3.1 | transaction | standard | needs: none
- 3.1 | asset | standard | needs: transaction
- 3.1 | liability | standard | needs: asset
- 3.1 | owner's equity | standard | needs: liability
- 3.2 | revenue | standard | needs: owner's equity
- 3.2 | expense | standard | needs: revenue
- 3.2 | drawings | standard | needs: expense
- 3.2 | equity in expanded form | standard | needs: drawings
- 3.3 | effect of a transaction | standard | needs: equity in expanded form
- 3.3 | analysing seven transactions | standard | needs: effect of a transaction
- 3.3 | checking the balance | standard | needs: analysing seven transactions
defines:
- Transaction | An event that has a money effect on a business and must be recorded.
- Asset | Something of value that a business owns or controls, such as cash, stock of goods or equipment.
- Liability | An amount a business owes to others, such as a loan or an unpaid supplier bill.
- Owner's equity | The owner's claim on the business: what is left of the assets after the liabilities are paid.
- Revenue | Money earned or becoming due from selling goods or doing services.
- Expense | A cost used up in earning revenue, such as rent or wages.
- Drawings | Money or goods the owner takes out of the business for personal use.
- Accounting equation | The rule that assets always equal liabilities plus owner's equity.
assumes: Accounting, Bookkeeping, Financial accounting
words-in-use: capital; asset
case: Meghna Binding Service, January 2026 transactions: 1 Jan owner invests cash Tk 500,000; 2 Jan buys shelving for cash Tk 120,000; 3 Jan buys book stock on credit from a publisher Tk 200,000; 10 Jan earns Tk 40,000 cash for binding work; 15 Jan pays rent Tk 60,000; 20 Jan pays Tk 80,000 to the publisher; 31 Jan pays wages Tk 35,000. At 31 January: cash Tk 245,000, shelving Tk 120,000, stock Tk 200,000 (assets Tk 565,000); payable Tk 120,000; equity Tk 445,000.
history: Medici Bank, from 1397: the Florentine bank kept accounts of its branches and partners, showing assets and obligations in the way the equation describes | search: Medici Bank 1397 accounting books
refs: Chapter 2, 'What Accounting Is', section 2.1 'The Idea of Accounting' — accounting; Chapter 2, 'What Accounting Is', section 2.3 'Rules and Concepts' — historical cost
need: you-will-need: Chapter 2 'What Accounting Is', section 2.1 'The Idea of Accounting'; Chapter 2 'What Accounting Is', section 2.3 'Rules and Concepts'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: classify items; fill missing figure in an equation; think: a transaction that changes only the asset side; pause: after the equation, after the expanded form
sources: no

=== CH04 ===
title: Accounts, Debits and Credits
purpose: Teaches the account, the six types of account and the double-entry rule of debit and credit.
pace: slow — double entry is new and abstract
style: calculation
words: 5275
figures: 2
- fig01: a T-account with left and right sides labelled
- fig02: six account types with their normal balance
plate: a Victorian clerk ruling a ledger page with a straight-edge and pen
sections:
4.1 | The Account | new idea | 425
4.2 | Six Types of Account | framework | 600
4.3 | Debit and Credit | defining theory | 1450
4.4 | Applying the Rules | technique | 200
concepts:
- 4.1 | account | standard | needs: none
- 4.1 | chart of accounts | minor | needs: account
- 4.1 | T-account | minor | needs: chart of accounts
- 4.2 | asset accounts | minor | needs: T-account
- 4.2 | liability accounts | minor | needs: asset accounts
- 4.2 | owner's capital account | minor | needs: liability accounts
- 4.2 | drawings account | minor | needs: owner's capital account
- 4.2 | revenue accounts | minor | needs: drawings account
- 4.2 | expense accounts | minor | needs: revenue accounts
- 4.3 | debit | minor | needs: expense accounts
- 4.3 | credit | minor | needs: debit
- 4.3 | normal balance | minor | needs: credit
- 4.3 | double entry | landmark | needs: normal balance
- 4.4 | rules for each account type | minor | needs: double entry
- 4.4 | recording seven transactions | minor | needs: rules for each account type
defines:
- Account | A record that collects all the increases and decreases of one kind of item, such as Cash.
- T-account | A simple account drawn as a letter T, with the debit side on the left and the credit side on the right.
- Debit | An entry on the left side of an account; it increases assets and expenses and decreases liabilities and equity.
- Credit | An entry on the right side of an account; it increases liabilities, equity and revenue and decreases assets.
- Double entry | The rule that every transaction is recorded twice, once as a debit and once as a credit of equal amount.
- Normal balance | The side of an account, debit or credit, on which increases are recorded and where its balance usually falls.
- Chart of accounts | A list of every account a business uses, with a code for each.
assumes: Transaction, Asset, Liability
words-in-use: debit; credit
case: The January 2026 transactions of Meghna Binding Service (see Chapter 3 case: cash 500,000 in; shelving 120,000; stock on credit 200,000; binding fee 40,000; rent 60,000; payment to publisher 80,000; wages 35,000). Final balances: Cash Dr 245,000; Shelving Dr 120,000; Stock Dr 200,000; Payable Cr 120,000; Capital Cr 500,000; Service Revenue Cr 40,000; Rent Dr 60,000; Wages Dr 35,000.
history: Luca Pacioli, 1494: Summa de arithmetica printed in Venice described the Venetian method of double-entry bookkeeping | search: Pacioli Summa 1494 double entry
refs: Chapter 3, 'The Accounting Equation', section 3.1 'Assets, Liabilities and Equity' — asset, liability, equity; Chapter 3, 'The Accounting Equation', section 3.2 'Revenue, Expenses and Drawings' — revenue and expense; Chapter 3, 'The Accounting Equation', section 3.3 'Transactions and the Equation' — transactions and the equation
need: you-will-need: Chapter 3 'The Accounting Equation', section 3.1 'Assets, Liabilities and Equity'; Chapter 3 'The Accounting Equation', section 3.2 'Revenue, Expenses and Drawings'; Chapter 3 'The Accounting Equation', section 3.3 'Transactions and the Equation'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: post new transactions to T-accounts; think: why cash increases on the debit side; pause: after normal balance, after double entry
sources: no

=== CH05 ===
title: The Journal and the Ledger
purpose: Shows how a transaction travels from a source document to the journal and then to the ledger, and names the steps of the accounting cycle.
pace: standard — procedure
style: calculation
words: 5275
figures: 2
- fig01: path from source document to journal to ledger
- fig02: the accounting cycle as a ring of steps
plate: a Victorian bookkeeper copying entries from a day-book into a large ledger
sections:
5.1 | The Accounting Cycle | new idea | 1000
5.2 | The Journal | technique | 900
5.3 | The Ledger and Posting | technique | 775
concepts:
- 5.1 | accounting cycle | core | needs: none
- 5.1 | steps in the cycle | standard | needs: accounting cycle
- 5.1 | source document | standard | needs: steps in the cycle
- 5.2 | journal | standard | needs: source document
- 5.2 | journal entry | standard | needs: journal
- 5.2 | narration | standard | needs: journal entry
- 5.2 | compound entry | standard | needs: narration
- 5.3 | ledger | standard | needs: compound entry
- 5.3 | posting | standard | needs: ledger
- 5.3 | balancing an account | standard | needs: posting
- 5.3 | folio references | minor | needs: balancing an account
defines:
- Accounting cycle | The series of steps repeated each period, from recording transactions to preparing closing records.
- Source document | Paper or electronic proof of a transaction, such as an invoice, receipt or cheque stub.
- Journal | The book of first entry, in which transactions are recorded in date order with debits and credits.
- Narration | A short note under a journal entry explaining the transaction.
- Compound entry | A journal entry that has more than one debit or more than one credit.
- Ledger | The book containing all of a business's accounts, where the effects of journal entries are collected.
- Posting | Copying the debits and credits from the journal to the right accounts in the ledger.
assumes: Account, T-account, Debit
words-in-use: book; folio
case: Journalising the seven January 2026 transactions of Meghna Binding Service (same figures as Chapter 3 case) with narrations, then posting to the ledger accounts Cash, Shelving, Stock, Publisher Payable, Capital, Service Revenue, Rent, Wages, and balancing Cash at Tk 245,000.
history: Francesco Datini, died 1410: merchant of Prato whose surviving archive holds hundreds of account books and ledgers | search: Datini archive Prato ledgers
refs: Chapter 4, 'Accounts, Debits and Credits', section 4.2 'Six Types of Account' — six account types; Chapter 4, 'Accounts, Debits and Credits', section 4.3 'Debit and Credit' — debit and credit
need: you-will-need: Chapter 4 'Accounts, Debits and Credits', section 4.2 'Six Types of Account'; Chapter 4 'Accounts, Debits and Credits', section 4.3 'Debit and Credit'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: journalise given transactions; post and balance; think: find the missing posting; pause: after the journal entry
sources: no

=== CH06 ===
title: The Trial Balance
purpose: Teaches how to list and total ledger balances to test equality, and what to do when the totals differ.
pace: standard — checking technique
style: calculation
words: 5275
figures: 1
- fig01: trial balance as a two-column list with totals
plate: a Victorian accountant checking two columns of figures with a ruler and magnifying glass
sections:
6.1 | Preparing a Trial Balance | technique | 1325
6.2 | What the Trial Balance Does Not Show | framework | 675
6.3 | Finding and Fixing Differences | technique | 675
concepts:
- 6.1 | trial balance | core | needs: none
- 6.1 | steps of preparation | standard | needs: trial balance
- 6.1 | the January trial balance | core | needs: steps of preparation
- 6.2 | errors that the trial balance finds | standard | needs: the January trial balance
- 6.2 | errors it cannot find | standard | needs: errors that the trial balance finds
- 6.2 | transposition error | standard | needs: errors it cannot find
- 6.3 | locating a difference | standard | needs: transposition error
- 6.3 | suspense account | standard | needs: locating a difference
- 6.3 | correcting entries | standard | needs: suspense account
defines:
- Trial balance | A list of all ledger account balances, with debits and credits totalled, to test that they are equal.
- Transposition error | A mistake in which two digits of a number are written in the wrong order, such as 53 for 35.
- Suspense account | A temporary account that holds a difference until its cause is found.
- Unadjusted trial balance | A trial balance taken before the period-end adjustments have been recorded.
assumes: Accounting cycle, Source document, Journal
words-in-use: balance; trial
case: Trial balance 31 January 2026, Meghna Binding Service: debits Cash 245,000; Shelving 120,000; Stock 200,000; Rent 60,000; Wages 35,000 = Tk 660,000; credits Publisher Payable 120,000; Capital 500,000; Service Revenue 40,000 = Tk 660,000. Error exercise: wages posted as 53,000 gives debit total 678,000, difference 18,000 (divisible by 9).
history: Simon Stevin, 1608: Dutch mathematician who wrote on bookkeeping for princes and proposed regular balancing of accounts | search: Simon Stevin 1608 bookkeeping
refs: Chapter 5, 'The Journal and the Ledger', section 5.3 'The Ledger and Posting' — ledger and posting; Chapter 5, 'The Journal and the Ledger', section 5.1 'The Accounting Cycle' — accounting cycle
need: you-will-need: Chapter 5 'The Journal and the Ledger', section 5.3 'The Ledger and Posting'; Chapter 5 'The Journal and the Ledger', section 5.1 'The Accounting Cycle'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: prepare a trial balance from balances; locate a difference; think: errors that do not unbalance; pause: after trial balance
sources: no

=== CH07 ===
title: Adjusting the Accounts
purpose: Explains why accounts need adjustment at period end and how each kind of adjustment is recorded.
pace: slow — accrual ideas are new and abstract
style: calculation
words: 5300
figures: 2
- fig01: timeline showing expense paid before and used up after
- fig02: four kinds of adjustment as quadrants
plate: a Victorian clerk at year-end with calendar, ledger and pile of unpaid bills
sections:
7.1 | Why Adjust | new idea | 900
7.2 | Deferrals | technique | 900
7.3 | Accruals | technique | 450
7.4 | Recording Adjusting Entries | technique | 450
concepts:
- 7.1 | accounting period | standard | needs: none
- 7.1 | accrual basis of accounting | standard | needs: accounting period
- 7.1 | matching principle | standard | needs: accrual basis of accounting
- 7.1 | reasons for adjustments | standard | needs: matching principle
- 7.2 | prepaid expense | standard | needs: reasons for adjustments
- 7.2 | supplies used | standard | needs: prepaid expense
- 7.2 | unearned revenue | standard | needs: supplies used
- 7.2 | depreciation entry | standard | needs: unearned revenue
- 7.3 | accrued expense | standard | needs: depreciation entry
- 7.3 | accrued revenue | standard | needs: accrued expense
- 7.4 | adjusting entry | standard | needs: accrued revenue
- 7.4 | the six adjustments for Meghna Binding Service | standard | needs: adjusting entry
defines:
- Accounting period | The span of time, such as a month or year, for which results are measured and reported.
- Accrual basis of accounting | Recording revenue when earned and expenses when incurred, whether or not cash has moved.
- Matching principle | The rule that expenses are recorded in the same period as the revenue they helped to earn.
- Prepaid expense | A cost paid in advance that is an asset until it is used up.
- Unearned revenue | Money received before work is done, held as a liability until the work is performed.
- Accrued expense | A cost already used up but not yet paid, shown as a liability.
- Accrued revenue | Revenue already earned but not yet received or billed, shown as an asset.
- Adjusting entry | A journal entry made at period end to bring an account to its correct balance for the period.
assumes: Trial balance, Transposition error, Suspense account, Account, T-account, Debit
words-in-use: accrual; period
case: Unadjusted trial balance 31 December 2026, Meghna Binding Service (Tk): debits Cash 150,000; Accounts Receivable 90,000; Supplies 48,000; Prepaid Insurance 36,000; Equipment 360,000; Drawings 80,000; Wages 330,000; Rent 120,000; Utilities 26,000 = 1,240,000; credits Accounts Payable 60,000; Unearned Revenue 24,000; Capital 400,000; Service Revenue 756,000 = 1,240,000. Adjustments: (a) supplies on hand Tk 15,000, so supplies expense 33,000; (b) insurance for 12 months paid 1 October 2026, Tk 3,000 a month, expired Tk 9,000; (c) equipment depreciation Tk 72,000 (5 years, no residual value); (d) unearned revenue earned Tk 12,000 (Tk 4,000 a month from 1 October); (e) wages owed Tk 14,000; (f) revenue earned but not billed Tk 20,000.
history: Enron, 2001: the energy company filed for bankruptcy on 2 December 2001 after accounting that hid debts, leading to the US Sarbanes-Oxley Act of 2002 | search: Enron bankruptcy 2 December 2001 Sarbanes-Oxley
refs: Chapter 6, 'The Trial Balance', section 6.1 'Preparing a Trial Balance' — trial balance; Chapter 4, 'Accounts, Debits and Credits', section 4.3 'Debit and Credit' — debit and credit
need: you-will-need: Chapter 6 'The Trial Balance', section 6.1 'Preparing a Trial Balance'; Chapter 4 'Accounts, Debits and Credits', section 4.3 'Debit and Credit'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: compute adjusting amounts for new data; think: what happens if no adjustment is made; pause: after matching principle, after accruals
sources: no

=== CH08 ===
title: The Worksheet
purpose: Teaches the ten-column worksheet from the unadjusted trial balance to the statements.
pace: slow — ten columns are a lot of structure to hold
style: calculation
words: 5175
figures: 1
- fig01: ten-column worksheet layout with column headings
plate: a Victorian clerk at a very wide ruled sheet with columns and an abacus
sections:
8.1 | Why Use a Worksheet | new idea | 1000
8.2 | Building the Ten Columns | technique | 900
8.3 | Worked Worksheet | technique | 675
concepts:
- 8.1 | worksheet | core | needs: none
- 8.1 | purposes of a worksheet | standard | needs: worksheet
- 8.1 | a working paper, not a statement | standard | needs: purposes of a worksheet
- 8.2 | trial balance columns | standard | needs: a working paper, not a statement
- 8.2 | adjustments columns | standard | needs: trial balance columns
- 8.2 | adjusted trial balance | standard | needs: adjustments columns
- 8.2 | statement columns | standard | needs: adjusted trial balance
- 8.3 | extending balances | standard | needs: statement columns
- 8.3 | totalling and finding net income | standard | needs: extending balances
- 8.3 | checking | standard | needs: totalling and finding net income
defines:
- Worksheet | A working paper with columns that moves figures from the trial balance through adjustments to the statements.
- Adjusted trial balance | A trial balance prepared after adjusting entries, showing the correct balance of each account.
- Net income | The amount by which total revenue exceeds total expenses for a period.
- Net loss | The amount by which total expenses exceed total revenue for a period.
assumes: Accounting period, Accrual basis of accounting, Matching principle, Trial balance, Transposition error, Suspense account
words-in-use: sheet; extend
case: Same dataset as Chapter 7 case: after adjustments Cash 150,000; Accounts Receivable 110,000; Supplies 15,000; Prepaid Insurance 27,000; Equipment 360,000; Accumulated Depreciation 72,000; Accounts Payable 60,000; Wages Payable 14,000; Unearned Revenue 12,000; Capital 400,000; Drawings 80,000; Service Revenue 788,000; Wages 344,000; Rent 120,000; Utilities 26,000; Supplies Expense 33,000; Insurance Expense 9,000; Depreciation Expense 72,000. Total expenses 604,000; net income 788,000 - 604,000 = Tk 184,000.
history: VisiCalc, 1979: Dan Bricklin and Bob Frankston wrote the first electronic spreadsheet for the Apple II, built from the accountant's columnar worksheet | search: VisiCalc 1979 Bricklin Frankston
refs: Chapter 7, 'Adjusting the Accounts', section 7.4 'Recording Adjusting Entries' — adjusting entries; Chapter 6, 'The Trial Balance', section 6.1 'Preparing a Trial Balance' — trial balance
need: you-will-need: Chapter 7 'Adjusting the Accounts', section 7.4 'Recording Adjusting Entries'; Chapter 6 'The Trial Balance', section 6.1 'Preparing a Trial Balance'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: complete a worksheet with new figures; think: a worksheet that does not balance; pause: after adjusted trial balance
sources: no

=== CH09 ===
title: Financial Statements and Closing Entries
purpose: Teaches the three end-of-period reports (profit, owner's capital, position), then closing and reversing entries.
pace: standard — end-of-cycle steps
style: calculation
words: 5175
figures: 2
- fig01: how the three statements link
- fig02: closing entries as arrows into capital
plate: a Victorian clerk sealing a finished annual account into a folder with a ribbon
sections:
9.1 | The Three Statements | new idea | 1000
9.2 | Closing the Books | technique | 900
9.3 | Reversing Entries | technique | 675
concepts:
- 9.1 | income statement | core | needs: none
- 9.1 | statement of owner's equity | standard | needs: income statement
- 9.1 | balance sheet | standard | needs: statement of owner's equity
- 9.2 | temporary account | standard | needs: balance sheet
- 9.2 | permanent account | standard | needs: temporary account
- 9.2 | closing entry | standard | needs: permanent account
- 9.2 | income summary | standard | needs: closing entry
- 9.3 | reversing entry | standard | needs: income summary
- 9.3 | when to reverse | standard | needs: reversing entry
- 9.3 | effect on next period | standard | needs: when to reverse
defines:
- Income statement | A report of revenue and expenses for a period, ending in net income or net loss.
- Statement of owner's equity | A report showing how the owner's capital changed during a period.
- Balance sheet | A report of assets, liabilities and owner's equity at one date.
- Temporary account | An account for revenue, expense or drawings, closed to zero at the end of each period.
- Permanent account | An account for assets, liabilities or capital, whose balance carries forward to the next period.
- Closing entry | A journal entry at period end that transfers a temporary account's balance to capital.
- Income summary | A temporary account used to collect revenue and expenses before the net result goes to capital.
- Reversing entry | An entry on the first day of a new period that cancels an earlier accrual so that routine payment can be recorded simply.
assumes: Worksheet, Adjusted trial balance, Net income, Accounting period, Accrual basis of accounting, Matching principle
words-in-use: statement; closing
case: Statements for 2026, Meghna Binding Service: revenue 788,000; expenses 604,000; net income 184,000; capital 1 January 2026 Tk 400,000 + 184,000 - drawings 80,000 = Tk 504,000 at 31 December; assets Tk 590,000 (cash 150,000, receivables 110,000, supplies 15,000, prepaid insurance 27,000, equipment net 288,000); liabilities Tk 86,000 (payables 60,000, wages payable 14,000, unearned revenue 12,000). Reversal 1 January 2027 of wages payable Tk 14,000; wages paid 5 January 2027 Tk 28,000.
history: Joint Stock Companies Act 1844 (United Kingdom): required registered companies to keep books and publish a full and fair balance sheet | search: Joint Stock Companies Act 1844 balance sheet
refs: Chapter 8, 'The Worksheet', section 8.3 'Worked Worksheet' — worksheet; Chapter 7, 'Adjusting the Accounts', section 7.2 'Deferrals' — deferrals
need: you-will-need: Chapter 8 'The Worksheet', section 8.3 'Worked Worksheet'; Chapter 7 'Adjusting the Accounts', section 7.2 'Deferrals'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: prepare statements from an adjusted trial balance; think: why revenue accounts close; pause: after closing entry
sources: no

=== CH10 ===
title: Accounts of a Merchandising Company
purpose: Shows how a business that buys and sells goods records sales and purchases, prepares classified statements and corrects errors.
pace: standard — new accounts and statement layouts
style: calculation
words: 5350
figures: 2
- fig01: gross profit as the gap between net sales and cost of goods sold
- fig02: the classified income statement layout
plate: a Victorian general store interior with shelves, barrels and a counter, 1880s
sections:
10.1 | Sales, Purchases and Returns | new idea | 900
10.2 | Classified Income Statement | technique | 675
10.3 | Classified Balance Sheet | technique | 675
10.4 | Correction of Errors | technique | 500
concepts:
- 10.1 | net sales | standard | needs: none
- 10.1 | sales return | standard | needs: net sales
- 10.1 | purchases and purchase return | standard | needs: sales return
- 10.1 | cost of goods sold | standard | needs: purchases and purchase return
- 10.2 | gross profit | standard | needs: cost of goods sold
- 10.2 | operating expense | standard | needs: gross profit
- 10.2 | selling and administrative expenses | standard | needs: operating expense
- 10.3 | current asset | standard | needs: selling and administrative expenses
- 10.3 | current liability | standard | needs: current asset
- 10.3 | non-current items | standard | needs: current liability
- 10.4 | error of omission | minor | needs: non-current items
- 10.4 | error of commission | minor | needs: error of omission
- 10.4 | error of principle | minor | needs: error of commission
- 10.4 | compensating error | minor | needs: error of principle
- 10.4 | correcting entries | minor | needs: compensating error
defines:
- Net sales | Sales less sales returns and allowances: the revenue a merchandising business really keeps.
- Cost of goods sold | The cost of the goods that were sold during the period.
- Gross profit | Net sales less cost of goods sold, before operating expenses are deducted.
- Operating expense | A cost of running the business, such as wages or rent, apart from the cost of goods sold.
- Current asset | An asset expected to be turned into cash or used up within one year.
- Current liability | A debt that must be paid within one year.
- Error of omission | A mistake in which a transaction is left out of the books completely.
- Error of principle | A mistake in which an entry is made in the wrong class of account, such as an expense recorded as an asset.
- Compensating error | Two or more mistakes that cancel each other so that the trial balance still agrees.
- Error of commission | A mistake in which an entry is made in the right class of account but the wrong account, or with a wrong amount.
assumes: Income statement, Statement of owner's equity, Balance sheet
words-in-use: sales; current
case: Meghna and Sons, 2025 (Tk): net sales 14,400,000; cost of goods sold: opening inventory 3,300,000 + purchases 10,200,000 - purchase returns 200,000 - closing inventory 3,500,000 = 9,800,000; gross profit 4,600,000; selling expenses 1,990,000; administrative expenses 1,860,000; total operating expenses 3,850,000; profit 750,000. Balance sheet 31 December 2025: current assets 4,260,000 (cash 420,000, receivables 280,000, inventory 3,500,000, prepaid 60,000); fittings net 600,000; total assets 4,860,000; current liabilities 1,270,000 (payables 1,150,000, loan due 120,000); long-term loan 480,000; capital 3,110,000 (opening 2,860,000 + profit 750,000 - drawings 500,000).
history: F. W. Woolworth, 1879: opened a first store in Utica, New York, which failed, then a fixed-price store in Lancaster, Pennsylvania, which succeeded | search: Woolworth 1879 Utica Lancaster five-and-ten
refs: Chapter 9, 'Financial Statements and Closing Entries', section 9.1 'The Three Statements' — the three statements; Chapter 9, 'Financial Statements and Closing Entries', section 9.2 'Closing the Books' — closing the books
need: you-will-need: Chapter 9 'Financial Statements and Closing Entries', section 9.1 'The Three Statements'; Chapter 9 'Financial Statements and Closing Entries', section 9.2 'Closing the Books'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: prepare a classified statement from new data; classify errors; think: a compensating error; pause: after cost of goods sold, after classified balance sheet
sources: no

=== CH11 ===
title: Special Journals
purpose: Shows how sales and purchases journals and control accounts save time when transactions are many.
pace: standard — system of books for repeated entries
style: calculation
words: 5050
figures: 1
- fig01: sales journal columns and posting to ledgers
plate: a Victorian warehouse office with several day-books on a sloping desk
sections:
11.1 | Why Special Journals | new idea | 775
11.2 | Sales and Purchases Journals | technique | 1000
11.3 | Control Accounts | technique | 675
concepts:
- 11.1 | special journal | core | needs: none
- 11.1 | the general journal's limits | standard | needs: special journal
- 11.2 | sales journal | core | needs: the general journal's limits
- 11.2 | purchases journal | standard | needs: sales journal
- 11.2 | posting totals | standard | needs: purchases journal
- 11.3 | control account | standard | needs: posting totals
- 11.3 | subsidiary ledger | standard | needs: control account
- 11.3 | agreeing totals | standard | needs: subsidiary ledger
defines:
- Special journal | A journal used for only one kind of repeated transaction, such as credit sales, to speed up recording.
- Sales journal | A special journal that records only sales of goods on credit.
- Purchases journal | A special journal that records only purchases of goods on credit.
- Subsidiary ledger | A ledger holding the individual accounts of customers or suppliers, supporting a main ledger account.
- Control account | A main ledger account that sums up the balances of all the accounts in a subsidiary ledger.
assumes: Accounting cycle, Source document, Journal
words-in-use: journal; control
case: November 2026, Meghna and Sons: credit sales 4 Nov Dhanmondi Girls' School Tk 85,000 (invoice 701); 12 Nov Rose Valley School Tk 120,000 (702); 20 Nov Lalmatia College Tk 64,000 (703); total Tk 269,000. Credit purchases 3 Nov Ananda Publishers Tk 150,000; 9 Nov Prothom Print Tk 96,000; 17 Nov Jonaki Books Tk 54,000; total Tk 300,000. Receivables control opening balance Tk 280,000.
history: Herman Hollerith, 1890: his punched-card tabulating machines processed the US census, starting the mechanisation of record-keeping | search: Hollerith 1890 census tabulating machine
refs: Chapter 5, 'The Journal and the Ledger', section 5.2 'The Journal' — the journal; Chapter 5, 'The Journal and the Ledger', section 5.3 'The Ledger and Posting' — ledger and posting
need: you-will-need: Chapter 5 'The Journal and the Ledger', section 5.2 'The Journal'; Chapter 5 'The Journal and the Ledger', section 5.3 'The Ledger and Posting'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: enter and post new credit sales; agree a control account; think: why not use special journals for a tiny business; pause: after control account
sources: no

=== CH12 ===
title: The Cash Book and Bank Reconciliation
purpose: Teaches the cash book and how to reconcile it with the bank's statement.
pace: standard — matching two sets of records
style: calculation
words: 5075
figures: 1
- fig01: two-sided reconciliation layout
plate: a Victorian bank counter with a teller, brass grille and a cash drawer
sections:
12.1 | The Cash Book | new idea | 675
12.2 | Why Bank and Book Differ | framework | 1125
12.3 | Preparing the Reconciliation | technique | 675
concepts:
- 12.1 | cash book | standard | needs: none
- 12.1 | cash and bank columns | standard | needs: cash book
- 12.1 | contra entry | standard | needs: cash and bank columns
- 12.2 | deposit in transit | standard | needs: contra entry
- 12.2 | outstanding cheque | standard | needs: deposit in transit
- 12.2 | credit memo | standard | needs: outstanding cheque
- 12.2 | debit memo | standard | needs: credit memo
- 12.2 | dishonoured cheque | standard | needs: debit memo
- 12.3 | bank reconciliation statement | standard | needs: dishonoured cheque
- 12.3 | adjusted balances | standard | needs: bank reconciliation statement
- 12.3 | entries after reconciling | standard | needs: adjusted balances
defines:
- Cash book | A book that serves as both journal and ledger for cash and bank receipts and payments.
- Bank reconciliation statement | A statement that explains the difference between the bank balance in the cash book and the balance on the bank's statement.
- Deposit in transit | Money recorded as deposited in the books but not yet shown on the bank statement.
- Outstanding cheque | A cheque written and recorded but not yet paid by the bank.
- Credit memo | A bank's notice that it has added money to the account, for example a collection or interest.
- Debit memo | A bank's notice that it has deducted money, for example a service charge.
- Dishonoured cheque | A cheque that the bank refuses to pay, usually because the writer's account lacks funds.
assumes: Special journal, Sales journal, Purchases journal, Accounting cycle, Source document, Journal
words-in-use: statement; clear
case: 31 October 2026: cash book bank balance Tk 612,000; bank statement balance Tk 655,000; deposit in transit Tk 48,000; outstanding cheques no. 4417 Tk 36,000 and no. 4421 Tk 22,000; bank service charge Tk 1,500; dishonoured customer cheque Tk 8,000; interest credited by bank Tk 2,500; school payment direct to bank not yet in books Tk 40,000. Adjusted balance 645,000 both ways.
history: London bankers' clearing house, from the 1770s: clerks of banks met to swap cheques and settle differences, the origin of cheque clearing | search: London bankers clearing house 1770s history
refs: Chapter 11, 'Special Journals', section 11.1 'Why Special Journals' — special journals; Chapter 5, 'The Journal and the Ledger', section 5.3 'The Ledger and Posting' — ledger and posting
need: you-will-need: Chapter 11 'Special Journals', section 11.1 'Why Special Journals'; Chapter 5 'The Journal and the Ledger', section 5.3 'The Ledger and Posting'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: prepare a reconciliation from new data; think: which items need journal entries; pause: after adjusted balances
sources: no

=== CH13 ===
title: Inventory: Meaning and Measurement
purpose: Explains inventory, why a business keeps it, and how its quantity and value are recorded.
pace: standard — new subject
style: mixed
words: 5175
figures: 1
- fig01: three kinds of inventory in a factory chain
plate: a Victorian warehouse with stacked crates, a hoist and a stock-taker with a list
sections:
13.1 | What Inventory Is | new idea | 1000
13.2 | Why Hold Inventory | framework | 675
13.3 | Recording Inventory | technique | 900
concepts:
- 13.1 | inventory | core | needs: none
- 13.1 | types of inventory | standard | needs: inventory
- 13.1 | raw materials, work in process, finished goods | standard | needs: types of inventory
- 13.2 | reasons to hold inventory | standard | needs: raw materials, work in process, finished goods
- 13.2 | cost of too little | standard | needs: reasons to hold inventory
- 13.2 | cost of too much | standard | needs: cost of too little
- 13.3 | perpetual inventory system | standard | needs: cost of too much
- 13.3 | periodic inventory system | standard | needs: perpetual inventory system
- 13.3 | stock count | standard | needs: periodic inventory system
- 13.3 | net realisable value | standard | needs: stock count
defines:
- Inventory | Goods held for sale, or materials held to make goods for sale, in the ordinary course of business.
- Raw materials | Materials a manufacturer holds to turn into products.
- Work in process | Goods in a factory that have been started but are not yet finished.
- Finished goods | Completed products held for sale.
- Perpetual inventory system | A system that updates the inventory account after every purchase and sale.
- Periodic inventory system | A system that finds inventory only by a physical count at period end.
- Net realisable value | The expected selling price of an item less the cost of selling it.
assumes: Net sales, Cost of goods sold, Gross profit
words-in-use: stock; cost
case: Meghna and Sons 31 December 2025: inventory at cost Tk 3,500,000; excess stock: 800 titles at cost Tk 380,000 unsold for 18 months, holding cost 10% a year = Tk 38,000; 120 copies of an old edition cost Tk 300 each, net realisable value Tk 180, write-down 120 x 120 = Tk 14,400.
history: Cisco Systems, April 2001: wrote down about USD 2.25 billion of excess inventory after demand fell, a famous case of too much stock | search: Cisco inventory writedown April 2001
refs: Chapter 10, 'Accounts of a Merchandising Company', section 10.1 'Sales, Purchases and Returns' — cost of goods sold; Chapter 10, 'Accounts of a Merchandising Company', section 10.3 'Classified Balance Sheet' — current assets
need: you-will-need: Chapter 10 'Accounts of a Merchandising Company', section 10.1 'Sales, Purchases and Returns'; Chapter 10 'Accounts of a Merchandising Company', section 10.3 'Classified Balance Sheet'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: classify items; compute a write-down; think: cost of holding too much; pause: after perpetual and periodic
sources: no

=== CH14 ===
title: Costing Inventory: FIFO, LIFO and Weighted Average
purpose: Teaches three cost flow assumptions and what each does to profit and inventory value.
pace: slow — three methods easily confused
style: calculation
words: 5175
figures: 2
- fig01: three methods as layers of cost leaving a pile
- fig02: results of the three methods side by side
plate: a Victorian cellar of casks in rows with a cooper marking the oldest
sections:
14.1 | The Problem of Changing Prices | new idea | 1000
14.2 | First-In First-Out | technique | 450
14.3 | Last-In First-Out | technique | 450
14.4 | Weighted Average Cost | technique | 675
concepts:
- 14.1 | cost flow assumption | core | needs: none
- 14.1 | ending inventory | standard | needs: cost flow assumption
- 14.1 | why method matters | standard | needs: ending inventory
- 14.2 | first-in first-out | standard | needs: why method matters
- 14.2 | worked case | standard | needs: first-in first-out
- 14.3 | last-in first-out | standard | needs: worked case
- 14.3 | worked case | standard | needs: last-in first-out
- 14.4 | weighted average cost | standard | needs: worked case
- 14.4 | worked case | standard | needs: weighted average cost
- 14.4 | comparing the three | standard | needs: worked case
defines:
- Cost flow assumption | A rule that decides which purchase costs are treated as sold and which remain in inventory.
- First-in first-out | A method that treats the oldest costs as sold first, leaving the newest costs in inventory.
- Last-in first-out | A method that treats the newest costs as sold first, leaving the oldest costs in inventory.
- Weighted average cost | A method that gives every unit the average cost of all units available.
- Ending inventory | The cost of goods still on hand at the end of the period.
assumes: Inventory, Raw materials, Work in process, Net sales, Cost of goods sold, Gross profit
words-in-use: flow; layer
case: Boxed dictionary sets: opening 20 units at Tk 300 (6,000); purchase 30 at Tk 320 (9,600); purchase 50 at Tk 340 (17,000); sold 70 units at Tk 500 = Tk 35,000; units available 100, cost Tk 32,600. FIFO: ending inventory 30 x 340 = 10,200; cost of goods sold 22,400; gross profit 12,600. LIFO: ending 20 x 300 + 10 x 320 = 9,200; cost of goods sold 23,400; gross profit 11,600. Weighted average: 32,600 / 100 = 326 a unit; ending 9,780; cost of goods sold 22,820; gross profit 12,180.
history: United States Revenue Act of 1939 allowed LIFO for tax purposes; International Accounting Standard 2 bars LIFO in countries following IFRS | search: LIFO Revenue Act 1939; IAS 2 LIFO prohibited
refs: Chapter 13, 'Inventory: Meaning and Measurement', section 13.3 'Recording Inventory' — perpetual and periodic; Chapter 10, 'Accounts of a Merchandising Company', section 10.1 'Sales, Purchases and Returns' — cost of goods sold
need: you-will-need: Chapter 13 'Inventory: Meaning and Measurement', section 13.3 'Recording Inventory'; Chapter 10 'Accounts of a Merchandising Company', section 10.1 'Sales, Purchases and Returns'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: compute all three on new data; think: rising and falling prices; pause: after FIFO, after weighted average
sources: no

=== CH15 ===
title: Plant Assets
purpose: Explains plant assets, what counts as their cost and the difference between capital and revenue spending.
pace: standard — new classification and cost rules
style: calculation
words: 5275
figures: 1
- fig01: cost build-up of one machine as stacked parts
plate: a Victorian machine shop with a lathe, belt drive and workmen
sections:
15.1 | What Plant Assets Are | new idea | 1000
15.2 | Cost of a Plant Asset | technique | 1000
15.3 | Capital and Revenue Spending | framework | 675
concepts:
- 15.1 | plant asset | core | needs: none
- 15.1 | intangible asset | standard | needs: plant asset
- 15.1 | nature of plant assets | standard | needs: intangible asset
- 15.2 | acquisition cost | core | needs: nature of plant assets
- 15.2 | items included | standard | needs: acquisition cost
- 15.2 | items excluded | standard | needs: items included
- 15.3 | capital expenditure | standard | needs: items excluded
- 15.3 | revenue expenditure | standard | needs: capital expenditure
- 15.3 | repairs and improvements | standard | needs: revenue expenditure
defines:
- Plant asset | A long-lived physical asset used in running a business, such as a machine or a building.
- Intangible asset | A long-lived asset with no physical form, such as a patent or a trademark.
- Acquisition cost | All the money spent to buy an asset and make it ready for use.
- Capital expenditure | Spending that creates or improves an asset lasting more than one period.
- Revenue expenditure | Spending that keeps an asset in working order and is treated as an expense of the period.
assumes: Income statement, Statement of owner's equity, Balance sheet, Accounting period, Accrual basis of accounting, Matching principle
words-in-use: plant; improvement
case: Book-cutting machine bought 1 April 2026: invoice Tk 300,000; freight Tk 10,000; installation Tk 15,000; trial run Tk 5,000; acquisition cost Tk 330,000; repair after a breakdown on 14 August 2026 Tk 8,000 (revenue expenditure).
history: WorldCom, 2002: the telecoms firm treated about USD 3.8 billion of line costs as assets instead of expenses, discovered in June 2002 | search: WorldCom 2002 capitalised line costs fraud
refs: Chapter 9, 'Financial Statements and Closing Entries', section 9.1 'The Three Statements' — the three statements; Chapter 7, 'Adjusting the Accounts', section 7.2 'Deferrals' — deferrals
need: you-will-need: Chapter 9 'Financial Statements and Closing Entries', section 9.1 'The Three Statements'; Chapter 7 'Adjusting the Accounts', section 7.2 'Deferrals'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: split a bill into capital and revenue; think: mis-classification effect; pause: after acquisition cost
sources: no

=== CH16 ===
title: Depreciation
purpose: Teaches why and how the cost of a plant asset is spread over its life, and what happens at sale.
pace: slow — abstract allocation idea with several methods
style: calculation
words: 5075
figures: 2
- fig01: straight-line against declining-balance as two descending lines
- fig02: book value stair-step by year
plate: a Victorian locomotive in a depot with an engineer inspecting wheels
sections:
16.1 | Why Depreciate | new idea | 900
16.2 | Straight-Line and Units-of-Production | technique | 450
16.3 | Declining-Balance | technique | 450
16.4 | Recording and Disposal | technique | 675
concepts:
- 16.1 | depreciation | standard | needs: none
- 16.1 | useful life | standard | needs: depreciation
- 16.1 | residual value | standard | needs: useful life
- 16.1 | factors affecting depreciation | standard | needs: residual value
- 16.2 | straight-line method | standard | needs: factors affecting depreciation
- 16.2 | units-of-production method | standard | needs: straight-line method
- 16.3 | declining-balance method | standard | needs: units-of-production method
- 16.3 | comparing the methods | standard | needs: declining-balance method
- 16.4 | accumulated depreciation | standard | needs: comparing the methods
- 16.4 | book value | standard | needs: accumulated depreciation
- 16.4 | sale of an asset | standard | needs: book value
defines:
- Depreciation | The spreading of a plant asset's cost over the years it is expected to be useful.
- Useful life | The period, or the amount of use, over which an asset is expected to serve the business.
- Residual value | The amount a business expects to receive when it disposes of an asset at the end of its useful life.
- Straight-line method | A depreciation method that charges an equal amount each year.
- Declining-balance method | A depreciation method that charges a fixed percentage of the asset's remaining book value each year.
- Units-of-production method | A depreciation method that charges an amount based on how much the asset is used in the period.
- Accumulated depreciation | The total depreciation charged to date on an asset, shown as a deduction from its cost.
- Book value | An asset's cost less its accumulated depreciation.
assumes: Plant asset, Intangible asset, Acquisition cost
words-in-use: value; life
case: Book-cutting machine, cost Tk 330,000 on 1 April 2026; residual Tk 30,000; life 5 years; capacity 500,000 books. Straight line: (330,000 - 30,000) / 5 = Tk 60,000 a year; 9 months of 2026 = Tk 45,000. Declining balance at 40%: year 1 = Tk 132,000, book value 198,000; year 2 = Tk 79,200, book value 118,800. Units of production: 300,000 / 500,000 = Tk 0.60 a book; 90,000 books in 2026 = Tk 54,000. Sale after three years of straight line: book value 330,000 - 180,000 = 150,000; sold for 120,000, loss Tk 30,000.
history: Dionysius Lardner, 1850: Railway Economy discussed the wearing out of locomotives and track, an early case for depreciation | search: Lardner Railway Economy 1850 depreciation
refs: Chapter 15, 'Plant Assets', section 15.2 'Cost of a Plant Asset' — acquisition cost; Chapter 15, 'Plant Assets', section 15.3 'Capital and Revenue Spending' — capital and revenue spending
need: you-will-need: Chapter 15 'Plant Assets', section 15.2 'Cost of a Plant Asset'; Chapter 15 'Plant Assets', section 15.3 'Capital and Revenue Spending'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: compute depreciation by each method; find gain or loss on sale; think: effect of a longer life; pause: after straight-line, after declining balance
sources: no

=== CH17 ===
title: Accounts of Clubs and Sole Traders
purpose: Shows the accounts of non-trading organisations and how a sole trader's profit is found when records are incomplete.
pace: standard — different records, same principles
style: mixed
words: 5275
figures: 1
- fig01: receipts and payments account turned into income and expenditure
plate: a Victorian village reading room with a committee table and a treasurer's box
sections:
17.1 | Non-Trading Organisations | new idea | 1325
17.2 | Income and Expenditure Account | technique | 675
17.3 | The Sole Trader from Incomplete Records | technique | 675
concepts:
- 17.1 | non-trading organisation | core | needs: none
- 17.1 | subscriptions | standard | needs: non-trading organisation
- 17.1 | receipts and payments account | core | needs: subscriptions
- 17.2 | income and expenditure account | standard | needs: receipts and payments account
- 17.2 | accrued and prepaid items | standard | needs: income and expenditure account
- 17.2 | club balance sheet | standard | needs: accrued and prepaid items
- 17.3 | statement of affairs | standard | needs: club balance sheet
- 17.3 | profit from changes in capital | standard | needs: statement of affairs
- 17.3 | sole trader's final accounts | standard | needs: profit from changes in capital
defines:
- Non-trading organisation | An organisation, such as a club or charity, that exists to serve its members or a cause rather than to earn profit.
- Receipts and payments account | A summary of all cash received and paid in a period, with opening and closing balances.
- Income and expenditure account | The non-trading organisation's equivalent of an income statement, showing surplus or deficit on an accrual basis.
- Statement of affairs | A list of a business's assets and liabilities at one date, used to find capital when records are incomplete.
- Subscription | A regular payment made by a member for belonging to an organisation.
assumes: Income statement, Statement of owner's equity, Balance sheet, Accounting period, Accrual basis of accounting, Matching principle
words-in-use: subscription; surplus
case: none
history: Oxfam, 1942: the Oxford Committee for Famine Relief was founded on 5 October 1942, a charity whose accounts show income and spending rather than profit | search: Oxford Committee for Famine Relief 1942
refs: Chapter 9, 'Financial Statements and Closing Entries', section 9.1 'The Three Statements' — the three statements; Chapter 7, 'Adjusting the Accounts', section 7.1 'Why Adjust' — accrual basis
need: you-will-need: Chapter 9 'Financial Statements and Closing Entries', section 9.1 'The Three Statements'; Chapter 7 'Adjusting the Accounts', section 7.1 'Why Adjust'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: convert a receipts and payments account; find profit by capital change; think: why a club has a deficit; pause: after income and expenditure
sources: no

=== CH18 ===
title: Partnership Accounts
purpose: Teaches how partners' capital, profit sharing, admission of a partner and liquidation are recorded.
pace: standard — new ownership with sharing rules
style: calculation
words: 5075
figures: 1
- fig01: profit sharing in layers: interest, then ratio
plate: a Victorian solicitor and three partners around a brass-bound table with a deed
sections:
18.1 | Forming a Partnership | new idea | 675
18.2 | Sharing Profit | technique | 675
18.3 | Admission of a Partner | technique | 450
18.4 | Liquidation | technique | 675
concepts:
- 18.1 | partnership | standard | needs: none
- 18.1 | partnership deed | standard | needs: partnership
- 18.1 | partner's capital | standard | needs: partnership deed
- 18.2 | profit-sharing ratio | standard | needs: partner's capital
- 18.2 | interest on capital | standard | needs: profit-sharing ratio
- 18.2 | appropriation account | standard | needs: interest on capital
- 18.3 | admission of a partner | standard | needs: appropriation account
- 18.3 | recording a new partner's capital | standard | needs: admission of a partner
- 18.4 | liquidation | standard | needs: recording a new partner's capital
- 18.4 | realisation | standard | needs: liquidation
- 18.4 | paying partners | standard | needs: realisation
defines:
- Partnership | A business owned by two or more people who agree to share its profit and loss.
- Partnership deed | A written agreement setting out each partner's capital, duties and share of profit.
- Profit-sharing ratio | The fixed proportion in which partners divide profit and loss.
- Interest on capital | A payment credited to partners for their capital before the remaining profit is divided.
- Liquidation | The process of selling a business's assets, paying its debts and sharing what remains among the owners.
- Realisation | The sale of assets, in liquidation, and the gain or loss on that sale.
assumes: Income statement, Statement of owner's equity, Balance sheet, Accounting cycle, Source document, Journal
words-in-use: capital; share
case: Deed dated 1 July 2026: Nurul Tk 2,500,000, Rafiq Tk 1,000,000, Tariq Tk 600,000; ratio 5:3:2; interest on capital 6% a year; profit 1 July to 31 December 2026 Tk 420,000: interest 75,000 + 30,000 + 18,000 = 123,000; remainder 297,000 shared 148,500, 89,100, 59,400; totals 223,500, 119,100, 77,400. Admission 1 January 2027 of Nusrat Meghna with cash Tk 700,000 credited as her capital. Liquidation 31 December 2027: assets book Tk 5,300,000 sold for Tk 4,400,000; liabilities Tk 1,200,000; loss Tk 900,000 shared 450,000, 270,000, 180,000; cash after debts Tk 3,200,000 paid as 2,050,000, 730,000, 420,000.
history: Goldman Sachs, 4 May 1999: the bank, a partnership since 1869, sold its shares to the public in an initial offering and became a company | search: Goldman Sachs IPO 4 May 1999 partnership
refs: Chapter 9, 'Financial Statements and Closing Entries', section 9.1 'The Three Statements' — statements; Chapter 5, 'The Journal and the Ledger', section 5.2 'The Journal' — journal
need: you-will-need: Chapter 9 'Financial Statements and Closing Entries', section 9.1 'The Three Statements'; Chapter 5 'The Journal and the Ledger', section 5.2 'The Journal'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: divide profit under a new deed; journalise admission; think: partner with a larger capital; pause: after interest on capital
sources: no

=== CH19 ===
title: Company Accounts: Shares
purpose: Teaches types of shares and how issuing shares is recorded.
pace: standard — new capital structure
style: calculation
words: 5175
figures: 1
- fig01: issue price split into face value and premium
plate: a Victorian company registrar's office with share certificates and a copying press
sections:
19.1 | Share Capital | new idea | 1000
19.2 | Types of Share | framework | 675
19.3 | Issuing Shares | technique | 900
concepts:
- 19.1 | share capital | core | needs: none
- 19.1 | authorised capital | standard | needs: share capital
- 19.1 | issued capital | standard | needs: authorised capital
- 19.2 | ordinary share | standard | needs: issued capital
- 19.2 | preference share | standard | needs: ordinary share
- 19.2 | rights of each | standard | needs: preference share
- 19.3 | share application | standard | needs: rights of each
- 19.3 | allotment | standard | needs: share application
- 19.3 | share premium | standard | needs: allotment
- 19.3 | journal entries for an issue | standard | needs: share premium
defines:
- Share capital | The money a company receives from shareholders in exchange for shares.
- Authorised capital | The largest amount of share capital a company is allowed to issue under its founding documents.
- Issued capital | The part of authorised capital that has actually been sold to shareholders.
- Ordinary share | A share that gives a holder a vote and a claim on profit after other claims are paid.
- Preference share | A share that gets a fixed dividend before ordinary shareholders, usually without a vote.
- Share application | A request to buy shares, sent with money, to a company that is offering them.
- Allotment | The company's formal decision to give shares to applicants.
- Share premium | The amount paid for a share above its face value.
assumes: Income statement, Statement of owner's equity, Balance sheet, Partnership, Partnership deed, Profit-sharing ratio
words-in-use: stock; issue
case: Meghna and Sons Limited (proposal): authorised capital Tk 10,000,000 in 1,000,000 shares of Tk 10; 410,000 shares issued to the partners for net assets of Tk 4,100,000; public issue of 200,000 shares at Tk 25 (face value Tk 10, premium Tk 15): application money Tk 10 a share = Tk 2,000,000; allotment Tk 15 a share = Tk 3,000,000; total Tk 5,000,000; share capital credited Tk 2,000,000; share premium credited Tk 3,000,000.
history: Joint Stock Companies Act 1856 (United Kingdom): allowed general limited liability for registered companies, which spread share ownership | search: Joint Stock Companies Act 1856 limited liability
refs: Chapter 9, 'Financial Statements and Closing Entries', section 9.3 'Reversing Entries' — balance sheet; Chapter 18, 'Partnership Accounts', section 18.1 'Forming a Partnership' — partnership
need: you-will-need: Chapter 9 'Financial Statements and Closing Entries', section 9.3 'Reversing Entries'; Chapter 18 'Partnership Accounts', section 18.1 'Forming a Partnership'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: journalise an issue; think: over-subscription; pause: after share premium
sources: no

=== CH20 ===
title: The Whole Cycle and Reporting Periods
purpose: Ties the cycle together for a trading business and explains reporting periods and cost classes.
pace: slow — review chapter ties many parts together
style: mixed
words: 5275
figures: 1
- fig01: how trial balance, adjustments, statements and closing connect
plate: a Victorian clerk and master at a year-end table with stacked finished ledgers
sections:
20.1 | Fiscal Year and Interim Periods | new idea | 1000
20.2 | Product Costs and Period Costs | framework | 775
20.3 | The Linked Statements | technique | 900
concepts:
- 20.1 | fiscal year | core | needs: none
- 20.1 | interim period | standard | needs: fiscal year
- 20.1 | calendar year | standard | needs: interim period
- 20.2 | product cost | core | needs: calendar year
- 20.2 | period cost | standard | needs: product cost
- 20.3 | how the statements connect | standard | needs: period cost
- 20.3 | payable versus receivable | standard | needs: how the statements connect
- 20.3 | trial balance versus balance sheet | standard | needs: payable versus receivable
- 20.3 | complete cycle review | standard | needs: trial balance versus balance sheet
defines:
- Fiscal year | Any twelve-month period a business chooses for its yearly reports, which may start in any month.
- Interim period | A reporting period shorter than a year, such as a quarter.
- Product cost | A cost of buying or making goods held for sale, such as the price paid for books and freight in.
- Period cost | A cost charged to the period in which it is incurred, such as rent or wages.
assumes: Income statement, Statement of owner's equity, Balance sheet, Net sales, Cost of goods sold, Gross profit, Cost flow assumption, First-in first-out
words-in-use: year; period
case: Meghna and Sons: calendar year ending 31 December; interim period quarter ended 30 September 2026, sales Tk 3,900,000; product costs: books bought Tk 2,450,000 and freight in Tk 40,000; period costs: rent Tk 180,000 and wages Tk 735,000. Bangladesh's government fiscal year runs from 1 July to 30 June.
history: United States Congressional Budget Act of 1974: moved the federal fiscal year to start on 1 October from 1976, a case of choosing a fiscal year | search: US federal fiscal year 1 October 1976
refs: Chapter 9, 'Financial Statements and Closing Entries', section 9.3 'Reversing Entries' — balance sheet; Chapter 10, 'Accounts of a Merchandising Company', section 10.2 'Classified Income Statement' — classified income statement; Chapter 14, 'Costing Inventory: FIFO, LIFO and Weighted Average', section 14.1 'The Problem of Changing Prices' — cost flow
need: you-will-need: Chapter 9 'Financial Statements and Closing Entries', section 9.3 'Reversing Entries'; Chapter 10 'Accounts of a Merchandising Company', section 10.2 'Classified Income Statement'; Chapter 14 'Costing Inventory: FIFO, LIFO and Weighted Average', section 14.1 'The Problem of Changing Prices'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: classify costs; link statements across a cycle; think: a different fiscal year; pause: after period cost
sources: no

=== A1 ===
title: Maths You Will Use
purpose: A short reference of the arithmetic this book uses, with one tiny worked example each.
pace: standard — reference, not a lesson
style: calculation
words: 1000
figures: 0
sections:
A1.1 | Percentages and Percentage Change | technique | 225
A1.2 | Ratios and Averages | technique | 225
A1.3 | Rearranging an Equation | technique | 225
A1.4 | Rounding and Units | technique | 100
A1.5 | Reading a Graph | technique | 225
concepts:
- A1.1 | percentage of a number and percentage change | standard | needs: none
- A1.2 | ratio and sharing in a ratio, simple and weighted average | standard | needs: percentage of a number
- A1.3 | moving a term across an equals sign | standard | needs: none
- A1.4 | rounding and keeping units | minor | needs: none
- A1.5 | axes, points, slope | standard | needs: none
defines: none
assumes: none
words-in-use: none
case: none
history: none (reference appendix)
refs: none
need: nothing
ladder: one tiny worked example per topic, then one two-step example
practice: none (reference appendix; answers and glossary files hold only the opening line)
sources: no
