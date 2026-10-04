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
course: MGT-104 Microeconomics
book: Microeconomics
spelling: British · dates: day, month, year (12 March 2026) · country: mixed, no single country; the running case is set in Bangladesh, examples and history draw on several countries, and the place is named whenever law or standards differ · currency and numbers: taka for the running case, written Tk 158,000 (international digit grouping); a historical case keeps its own currency
running case: Meghna and Sons — illustrative documented case file. Base facts (FIXED): Meghna and Sons, a family-run bookshop in Dhanmondi, Dhaka, founded 4 April 1996 by Nurul Meghna. 2025: sales Tk 14,400,000; profit Tk 750,000; capital employed Tk 4,100,000; rent Tk 60,000 a month; 7 staff, total wages Tk 2,940,000; bank loan Tk 600,000. A popular 200-page notebook is the shop's example good in demand and supply chapters. Each chapter's case slice states its own figures.
chosen words (one term, one word):
- use good — also called commodity, product (service when no physical thing)
- use price — also called cost to the buyer (cost means the seller's expense)
- use consumer — also called buyer who uses a good
- use firm — also called business, enterprise (one word: firm)
- use quantity demanded — also called demand (demand alone means the whole schedule)
- use shift — also called change in demand or supply (movement along is a separate word)
size plan: 16 chapters plus appendix A1, ~80450 words, ~321 pages (target 320)
chapters:
1. What Economics Is
2. Scarcity, Choice and Opportunity Cost
3. Utility and Diminishing Marginal Utility
4. Demand and the Demand Curve
5. Elasticity of Demand
6. Consumption and Consumers' Surplus
7. Indifference Curve Analysis
8. Supply and the Supply Curve
9. Elasticity of Supply and Market Equilibrium
10. Production Function and Returns
11. Costs of Production
12. The Least-Cost Combination of Inputs
13. Market Structure and Perfect Competition
14. Monopoly and Imperfect Competition
15. Rent and Wages
16. Interest and Profit
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
title: What Economics Is
purpose: Introduces economics, its two main branches, its method and its laws.
pace: slow — first chapter; teaches the book's habits and the subject's vocabulary
style: descriptive
words: 5000
figures: 2
- fig01: economics as choice among wants and limited means
- fig02: micro and macro viewpoints on one shop
plate: a Victorian market square with stalls, shoppers and a clock tower, 1880s
sections:
1.1 | The Meaning and Scope of Economics | new idea | 675
1.2 | Micro and Macro | framework | 900
1.3 | Positive and Normative Statements | framework | 425
1.4 | Economic Laws | framework | 400
concepts:
- 1.1 | economics | standard | needs: none
- 1.1 | scope of economics | standard | needs: economics
- 1.1 | economics as science or art | standard | needs: scope of economics
- 1.2 | microeconomics | standard | needs: economics as science or art
- 1.2 | macroeconomics | standard | needs: microeconomics
- 1.2 | differences and links | standard | needs: macroeconomics
- 1.2 | scope of microeconomics | standard | needs: differences and links
- 1.3 | positive economics | standard | needs: scope of microeconomics
- 1.3 | normative economics | minor | needs: positive economics
- 1.3 | telling them apart | minor | needs: normative economics
- 1.4 | economic law | minor | needs: telling them apart
- 1.4 | ceteris paribus | minor | needs: economic law
- 1.4 | how economic laws differ from natural laws | minor | needs: ceteris paribus
- 1.4 | basic problems of an economy | minor | needs: how economic laws differ from natural laws
defines:
- Economics | The study of how people, firms and governments choose to use limited resources to satisfy wants.
- Microeconomics | The branch of economics that studies choices made by individual consumers and firms.
- Macroeconomics | The branch of economics that studies the whole economy, including output, prices and employment.
- Positive economics | Statements about what is, which can be tested against facts.
- Normative economics | Statements about what ought to be, which depend on values and cannot be tested by facts alone.
- Economic law | A statement of how people usually behave, true when other conditions stay the same.
- Ceteris paribus | A Latin phrase meaning all other things stay the same, used to study one change at a time.
assumes: none
words-in-use: economy; market
case: The shop as a micro unit: 2025 sales Tk 14,400,000, profit Tk 750,000, 7 staff; macro view: the price level and total employment of the whole country. Statements: 'The shop sold 41,000 copies in 2025' (positive); 'Textbooks should be cheaper' (normative).
history: Alfred Marshall, 1890: Principles of Economics defined economics as the study of people in the ordinary business of life | search: Marshall 1890 Principles of Economics definition
refs: none
need: nothing
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: classify statements; separate micro from macro questions; think: why economic laws need ceteris paribus; pause: after positive and normative
sources: no

=== CH02 ===
title: Scarcity, Choice and Opportunity Cost
purpose: Builds the idea that every choice gives something up, and measures what is given up.
pace: slow — opportunity cost is new and abstract
style: mixed
words: 4950
figures: 1
- fig01: a production possibility curve with three points
plate: a Victorian traveller at a fork in the road with two signposts
sections:
2.1 | Wants and Scarcity | new idea | 1000
2.2 | Opportunity Cost | new idea | 675
2.3 | The Production Possibility Curve | technique | 675
concepts:
- 2.1 | want | standard | needs: none
- 2.1 | scarcity | core | needs: want
- 2.1 | resources and choice | standard | needs: scarcity
- 2.2 | opportunity cost | standard | needs: resources and choice
- 2.2 | trade-off | standard | needs: opportunity cost
- 2.2 | measuring opportunity cost | standard | needs: trade-off
- 2.3 | production possibility curve | standard | needs: measuring opportunity cost
- 2.3 | reading the curve | standard | needs: production possibility curve
- 2.3 | growth shifts the curve | standard | needs: reading the curve
defines:
- Want | Something a person would like to have, however little or much the person can pay for it.
- Scarcity | The condition that resources are limited while wants are unlimited, so choices must be made.
- Opportunity cost | The value of the best alternative given up when a choice is made.
- Trade-off | Giving up some of one thing in order to have more of another.
- Production possibility curve | A line showing the largest combinations of two goods that can be produced with all resources used fully.
assumes: Economics, Microeconomics, Macroeconomics
words-in-use: cost; choice
case: Rafiq has 40 hours a week for two jobs: shelving 20 books an hour or packing 10 online parcels an hour. All 40 hours on shelving = 800 books, 0 parcels; all on parcels = 400 parcels, 0 books. Each extra parcel gives up 2 books. Nurul's Tk 500,000: new titles or an online site; opportunity cost of one is the other.
history: Lionel Robbins, 1932: An Essay on the Nature and Significance of Economic Science defined economics as the study of choice under scarcity | search: Robbins 1932 Essay Nature Significance Economic Science
refs: Chapter 1, 'What Economics Is', section 1.1 'The Meaning and Scope of Economics' — economics defined; Chapter 1, 'What Economics Is', section 1.4 'Economic Laws' — laws and the same-conditions rule
need: you-will-need: Chapter 1 'What Economics Is', section 1.1 'The Meaning and Scope of Economics'; Chapter 1 'What Economics Is', section 1.4 'Economic Laws'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: compute opportunity cost from a table; think: the cost of studying an hour; pause: after opportunity cost
sources: no

=== CH03 ===
title: Utility and Diminishing Marginal Utility
purpose: Explains satisfaction as a measurable idea and the law that each extra unit adds less.
pace: slow — abstract and layered
style: mixed
words: 5075
figures: 2
- fig01: total utility rising and levelling
- fig02: marginal utility falling and crossing zero
plate: a Victorian tea table with a diner finishing the third cup, scene of plenty
sections:
3.1 | The Idea of Utility | new idea | 900
3.2 | The Law of Diminishing Marginal Utility | defining theory | 1375
3.3 | Limits of the Law | framework | 200
concepts:
- 3.1 | utility | standard | needs: none
- 3.1 | total utility | standard | needs: utility
- 3.1 | marginal utility | standard | needs: total utility
- 3.1 | measuring satisfaction | standard | needs: marginal utility
- 3.2 | law of diminishing marginal utility | landmark | needs: measuring satisfaction
- 3.2 | the schedule and the curve | standard | needs: law of diminishing marginal utility
- 3.3 | assumptions | minor | needs: the schedule and the curve
- 3.3 | exceptions and limitations | minor | needs: assumptions
defines:
- Utility | The satisfaction a person gets from using a good or service.
- Total utility | The whole satisfaction from all units of a good consumed.
- Marginal utility | The extra satisfaction from consuming one more unit of a good.
- Law of diminishing marginal utility | As a person consumes more units of a good, the satisfaction added by each extra unit falls.
- Util | An imaginary unit used to count satisfaction in economic examples.
assumes: Want, Scarcity, Opportunity cost, Economics, Microeconomics, Macroeconomics
words-in-use: utility; marginal
case: A student buying exercise books at the shop: marginal utility in utils: 1st = 40, 2nd = 30, 3rd = 20, 4th = 10, 5th = 0; total utility 40, 70, 90, 100, 100.
history: Marginal revolution, 1871 to 1874: Jevons in England, Menger in Austria and Walras in Switzerland each developed marginal utility; Gossen had stated it in 1854 | search: marginal revolution 1871 Jevons Menger Walras
refs: Chapter 2, 'Scarcity, Choice and Opportunity Cost', section 2.2 'Opportunity Cost' — opportunity cost; Chapter 1, 'What Economics Is', section 1.4 'Economic Laws' — laws
need: you-will-need: Chapter 2 'Scarcity, Choice and Opportunity Cost', section 2.2 'Opportunity Cost'; Chapter 1 'What Economics Is', section 1.4 'Economic Laws'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: complete a utility table; think: why a fifth cup may have zero utility; pause: after marginal utility
sources: no

=== CH04 ===
title: Demand and the Demand Curve
purpose: Builds the law of demand, the curve, its shifts and its exceptions.
pace: slow — demand curve is the book's central diagram
style: mixed
words: 5025
figures: 3
- fig01: demand curve with a movement along it
- fig02: demand curve with a shift
- fig03: a Giffen-type upward curve
plate: a Victorian fishmonger's stall with price tablets and a crowd of buyers
sections:
4.1 | Quantity Demanded and the Law of Demand | new idea | 900
4.2 | Movement and Shift | framework | 675
4.3 | Related and Unusual Goods | framework | 850
concepts:
- 4.1 | demand | standard | needs: none
- 4.1 | demand schedule | standard | needs: demand
- 4.1 | law of demand | standard | needs: demand schedule
- 4.1 | demand curve | standard | needs: law of demand
- 4.2 | movement along the curve | standard | needs: demand curve
- 4.2 | determinants of demand | standard | needs: movement along the curve
- 4.2 | shift of the curve | standard | needs: determinants of demand
- 4.3 | substitute goods | standard | needs: shift of the curve
- 4.3 | complementary goods | standard | needs: substitute goods
- 4.3 | normal good | minor | needs: complementary goods
- 4.3 | Giffen good | minor | needs: normal good
- 4.3 | snob effect | minor | needs: Giffen good
- 4.3 | bandwagon effect | minor | needs: snob effect
defines:
- Demand | The quantities of a good that buyers are willing and able to buy at each price in a period.
- Law of demand | When the price of a good rises, the quantity demanded falls, if other conditions stay the same.
- Demand schedule | A table showing the quantity demanded at each price.
- Demand curve | A line on a graph showing the quantity demanded at each price.
- Normal good | A good for which demand rises when buyers' incomes rise.
- Substitute goods | Goods that can replace each other, so a price rise for one raises demand for the other.
- Complementary goods | Goods used together, so a price rise for one lowers demand for the other.
- Giffen good | A rare, very basic good for which quantity demanded rises when its price rises.
- Snob effect | A fall in demand for a good because many other people now buy it.
- Bandwagon effect | A rise in demand for a good because many other people buy it.
assumes: Utility, Total utility, Marginal utility, Economics, Microeconomics, Macroeconomics
words-in-use: demand; normal
case: Weekly demand for the shop's 200-page notebook: price Tk 60 → 20; Tk 50 → 30; Tk 40 → 40; Tk 30 → 50; Tk 20 → 60 (quantity = 80 - price). Fall in the price of a rival notebook of Tk 10 moves weekly demand at every price down by 5.
history: Antoine Cournot, 1838: Researches into the Mathematical Principles of the Theory of Wealth set out demand as a function of price | search: Cournot 1838 demand function Researches
refs: Chapter 3, 'Utility and Diminishing Marginal Utility', section 3.2 'The Law of Diminishing Marginal Utility' — diminishing utility; Chapter 1, 'What Economics Is', section 1.4 'Economic Laws' — ceteris paribus
need: you-will-need: Chapter 3 'Utility and Diminishing Marginal Utility', section 3.2 'The Law of Diminishing Marginal Utility'; Chapter 1 'What Economics Is', section 1.4 'Economic Laws'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: plot and shift a curve; classify price changes as movement or shift; think: a luxury that sells more as price rises; pause: after law of demand, after shift
sources: no

=== CH05 ===
title: Elasticity of Demand
purpose: Teaches how to measure how strongly buyers respond to price, income and rival prices.
pace: slow — first measured quantity, easily mixed up
style: calculation
words: 4975
figures: 2
- fig01: elastic and inelastic demand curves
- fig02: total revenue test as a table graph
plate: a Victorian pharmacist's scales weighing a small and a large weight on a see-saw
sections:
5.1 | Price Elasticity of Demand | technique | 1475
5.2 | Reading the Result | framework | 400
5.3 | Factors Behind Elasticity | framework | 300
5.4 | Income and Cross Elasticity | technique | 200
concepts:
- 5.1 | price elasticity of demand | landmark | needs: none
- 5.1 | the calculation | standard | needs: price elasticity of demand
- 5.1 | midpoint method | minor | needs: the calculation
- 5.2 | elastic demand | minor | needs: midpoint method
- 5.2 | inelastic demand | minor | needs: elastic demand
- 5.2 | unit elastic demand | minor | needs: inelastic demand
- 5.2 | total revenue test | minor | needs: unit elastic demand
- 5.3 | substitutes | minor | needs: total revenue test
- 5.3 | necessity and luxury | minor | needs: substitutes
- 5.3 | time and share of income | minor | needs: necessity and luxury
- 5.4 | income elasticity | minor | needs: time and share of income
- 5.4 | cross elasticity | minor | needs: income elasticity
defines:
- Price elasticity of demand | The percentage change in quantity demanded divided by the percentage change in price.
- Elastic demand | Demand for which the percentage change in quantity is larger than the percentage change in price.
- Inelastic demand | Demand for which the percentage change in quantity is smaller than the percentage change in price.
- Unit elastic demand | Demand for which the percentage change in quantity equals the percentage change in price.
- Income elasticity | The percentage change in quantity demanded divided by the percentage change in buyers' income.
- Cross elasticity | The percentage change in quantity demanded of one good divided by the percentage change in the price of another.
- Total revenue | Price multiplied by quantity sold.
assumes: Demand, Law of demand, Demand schedule
words-in-use: elastic; response
case: From the notebook schedule (quantity = 80 - price): price falls from Tk 50 to Tk 40, quantity rises from 30 to 40. Midpoint: percentage change in quantity = 10 / 35 = 28.57%; in price = 10 / 45 = 22.22%; elasticity = 1.29 (elastic). Total revenue Tk 1,500 → Tk 1,600. Income rise of 10% lifts quantity of fiction by 15%: income elasticity 1.5. Rival notebook price up 10% raises quantity by 4%: cross elasticity 0.4.
history: OPEC oil embargo, October 1973: oil prices roughly quadrupled and rich countries' demand fell only slowly, showing low short-run elasticity | search: 1973 oil embargo price quadrupled demand elasticity
refs: Chapter 4, 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand' — demand and its curve; Chapter 4, 'Demand and the Demand Curve', section 4.2 'Movement and Shift' — movement and shift
need: you-will-need: Chapter 4 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand'; Chapter 4 'Demand and the Demand Curve', section 4.2 'Movement and Shift'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: compute elasticity with new data; use the revenue test; think: a firm with inelastic demand; pause: after midpoint calculation
sources: no

=== CH06 ===
title: Consumption and Consumers' Surplus
purpose: Shows how a buyer gains more than the price paid, and how to measure the gain.
pace: standard — framework applied to numbers
style: mixed
words: 5050
figures: 1
- fig01: consumers' surplus as the triangle under the demand curve
plate: a Victorian engraving of a market-day bargain, a buyer counting coins beside a stall
sections:
6.1 | Consumption and Choice | new idea | 775
6.2 | Consumers' Surplus | framework | 1000
6.3 | Using the Idea | framework | 675
concepts:
- 6.1 | consumption | core | needs: none
- 6.1 | how buyers spend income | standard | needs: consumption
- 6.2 | willingness to pay | standard | needs: how buyers spend income
- 6.2 | consumers' surplus | core | needs: willingness to pay
- 6.2 | measuring the surplus | standard | needs: consumers' surplus
- 6.3 | practical uses | standard | needs: measuring the surplus
- 6.3 | limits of the idea | standard | needs: practical uses
- 6.3 | price changes and surplus | standard | needs: limits of the idea
defines:
- Consumption | The use of goods and services by people to satisfy their wants.
- Willingness to pay | The highest price a buyer would pay for one more unit of a good.
- Consumers' surplus | The gap between what buyers are willing to pay for a good and what they actually pay.
assumes: Utility, Total utility, Marginal utility, Demand, Law of demand, Demand schedule
words-in-use: surplus; spend
case: Five regular customers' willingness to pay for the notebook: Tk 60, 50, 40, 30, 20; market price Tk 40; those who buy: three; consumers' surplus = (60 - 40) + (50 - 40) + (40 - 40) = Tk 30. If price falls to Tk 30: surplus = 30 + 20 + 10 + 0 = Tk 60.
history: Jules Dupuit, 1844: a French engineer who wrote on the utility of public works and showed that people gain more from a bridge than the toll they pay | search: Dupuit 1844 utility of public works consumer surplus
refs: Chapter 3, 'Utility and Diminishing Marginal Utility', section 3.1 'The Idea of Utility' — marginal utility; Chapter 4, 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand' — the demand curve
need: you-will-need: Chapter 3 'Utility and Diminishing Marginal Utility', section 3.1 'The Idea of Utility'; Chapter 4 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: compute surplus for new prices; think: why the surplus rises when price falls; pause: after willingness to pay
sources: no

=== CH07 ===
title: Indifference Curve Analysis
purpose: Teaches the indifference curve, the budget line and the point where a buyer is best off.
pace: slow — graph with three lines at once
style: mixed
words: 4950
figures: 2
- fig01: an indifference curve convex to the origin
- fig02: a budget line and tangency point
plate: a Victorian shopper in a draper's weighing two bolts of cloth against a purse
sections:
7.1 | Indifference Curves | defining theory | 1000
7.2 | Marginal Rate of Substitution | technique | 450
7.3 | The Budget Line | technique | 450
7.4 | Consumer Equilibrium | defining theory | 450
concepts:
- 7.1 | indifference curve | core | needs: none
- 7.1 | indifference map | standard | needs: indifference curve
- 7.1 | properties of the curves | standard | needs: indifference map
- 7.2 | marginal rate of substitution | standard | needs: properties of the curves
- 7.2 | diminishing rate | standard | needs: marginal rate of substitution
- 7.3 | budget line | standard | needs: diminishing rate
- 7.3 | shifts and rotations of the line | standard | needs: budget line
- 7.4 | consumer equilibrium | standard | needs: shifts and rotations of the line
- 7.4 | tangency condition | standard | needs: consumer equilibrium
defines:
- Indifference curve | A line showing all combinations of two goods that give a consumer the same satisfaction.
- Indifference map | A set of indifference curves, with higher curves showing greater satisfaction.
- Marginal rate of substitution | The amount of one good a consumer will give up to gain one more unit of another good.
- Budget line | A line showing every combination of two goods that a consumer can buy with a given income at given prices.
- Consumer equilibrium | The point where a consumer gets the greatest satisfaction possible from a given income.
assumes: Utility, Total utility, Marginal utility, Consumption, Willingness to pay, Consumers' surplus
words-in-use: curve; line
case: A student has Tk 400 for notebooks at Tk 40 and pens at Tk 20: maximum 10 notebooks or 20 pens. Best bundle: 6 notebooks and 8 pens (240 + 160 = 400), where the marginal rate of substitution is 2, equal to the price ratio 40 / 20 = 2.
history: John Hicks and R. G. D. Allen, 1934: A Reconsideration of the Theory of Value rebuilt consumer theory on indifference curves | search: Hicks Allen 1934 Reconsideration Theory of Value
refs: Chapter 3, 'Utility and Diminishing Marginal Utility', section 3.1 'The Idea of Utility' — utility; Chapter 6, 'Consumption and Consumers' Surplus', section 6.2 'Consumers' Surplus' — consumers' surplus
need: you-will-need: Chapter 3 'Utility and Diminishing Marginal Utility', section 3.1 'The Idea of Utility'; Chapter 6 'Consumption and Consumers' Surplus', section 6.2 'Consumers' Surplus'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: draw a budget line; find the best bundle; think: effect of a price fall; pause: after marginal rate of substitution
sources: no

=== CH08 ===
title: Supply and the Supply Curve
purpose: Builds the law of supply, the supply curve, and its shifts and exceptions.
pace: standard — mirrors demand
style: mixed
words: 5050
figures: 2
- fig01: supply curve with movement and shift
- fig02: backward-bending supply curve
plate: a Victorian farm-cart arriving at market loaded with sacks and crates
sections:
8.1 | Quantity Supplied and the Law of Supply | new idea | 900
8.2 | Determinants of Supply | framework | 1125
8.3 | Exceptional Supply Curves | framework | 425
concepts:
- 8.1 | supply | standard | needs: none
- 8.1 | supply schedule | standard | needs: supply
- 8.1 | law of supply | standard | needs: supply schedule
- 8.1 | supply curve | standard | needs: law of supply
- 8.2 | cost of inputs | standard | needs: supply curve
- 8.2 | technology | standard | needs: cost of inputs
- 8.2 | taxes and subsidies | standard | needs: technology
- 8.2 | number of sellers | standard | needs: taxes and subsidies
- 8.2 | shift of the supply curve | standard | needs: number of sellers
- 8.3 | exceptional supply curve | standard | needs: shift of the supply curve
- 8.3 | backward-bending curve | minor | needs: exceptional supply curve
- 8.3 | fixed supply | minor | needs: backward-bending curve
defines:
- Supply | The quantities of a good that sellers are willing and able to sell at each price in a period.
- Law of supply | When the price of a good rises, the quantity supplied rises, if other conditions stay the same.
- Supply schedule | A table showing the quantity supplied at each price.
- Supply curve | A line on a graph showing the quantity supplied at each price.
- Exceptional supply curve | A supply curve that does not slope upward, such as one that bends backward or stays fixed.
assumes: Demand, Law of demand, Demand schedule
words-in-use: supply; cost
case: Weekly supply of the notebook by its wholesaler: price Tk 20 → 20; Tk 30 → 30; Tk 40 → 40; Tk 50 → 50; Tk 60 → 60 (quantity supplied = price). A tax on paper that lowers supply by 10 at every price.
history: Alfred Marshall, 1890: compared supply and demand to the two blades of a pair of scissors, both needed to cut | search: Marshall scissors supply demand 1890
refs: Chapter 4, 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand' — demand; Chapter 4, 'Demand and the Demand Curve', section 4.2 'Movement and Shift' — shifts
need: you-will-need: Chapter 4 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand'; Chapter 4 'Demand and the Demand Curve', section 4.2 'Movement and Shift'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: plot and shift a supply curve; sort price and non-price causes; think: a worker who works fewer hours at very high pay; pause: after law of supply
sources: no

=== CH09 ===
title: Elasticity of Supply and Market Equilibrium
purpose: Measures how sellers respond to price, then joins demand and supply to find the market price.
pace: standard — bring both sides together
style: calculation
words: 4850
figures: 2
- fig01: market equilibrium crossing
- fig02: shortage and excess supply at wrong prices
plate: a Victorian corn exchange with merchants round a pit and a bell
sections:
9.1 | Elasticity of Supply | technique | 900
9.2 | Market Equilibrium | framework | 675
9.3 | Changes in Equilibrium | technique | 675
concepts:
- 9.1 | elasticity of supply | standard | needs: none
- 9.1 | measuring it | standard | needs: elasticity of supply
- 9.1 | types of elasticity of supply | standard | needs: measuring it
- 9.1 | factors | standard | needs: types of elasticity of supply
- 9.2 | competitive market | standard | needs: factors
- 9.2 | equilibrium price | standard | needs: competitive market
- 9.2 | excess demand and excess supply | standard | needs: equilibrium price
- 9.3 | demand shift | standard | needs: excess demand and excess supply
- 9.3 | supply shift | standard | needs: demand shift
- 9.3 | both shifts | standard | needs: supply shift
defines:
- Elasticity of supply | The percentage change in quantity supplied divided by the percentage change in price.
- Competitive market | A market with many buyers and sellers, none of whom can set the price.
- Equilibrium price | The price at which the quantity buyers want equals the quantity sellers offer.
- Excess demand | A situation in which buyers want more than sellers offer at a price, pushing the price up.
- Excess supply | A situation in which sellers offer more than buyers want at a price, pushing the price down.
assumes: Demand, Law of demand, Demand schedule, Supply, Law of supply, Supply schedule, Price elasticity of demand, Elastic demand
words-in-use: equilibrium; market
case: Notebook market: demand quantity = 80 - price; supply quantity = price; equilibrium price Tk 40, quantity 40. At Tk 50: demand 30, supply 50, excess supply 20; at Tk 30: demand 50, supply 30, excess demand 20. Elasticity of supply between Tk 30 and Tk 40 (midpoint): 10 / 35 over 10 / 35 = 1.0. Demand rises to 100 - price: new equilibrium Tk 50, quantity 50. Supply falls to price - 10 with original demand: Tk 45, quantity 35.
history: Bangladesh onion price spike, September to November 2019: after India halted onion exports on 29 September 2019, prices in Dhaka rose sharply | search: India onion export ban 29 September 2019 Bangladesh price
refs: Chapter 4, 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand' — demand curve; Chapter 8, 'Supply and the Supply Curve', section 8.1 'Quantity Supplied and the Law of Supply' — supply curve; Chapter 5, 'Elasticity of Demand', section 5.1 'Price Elasticity of Demand' — elasticity
need: you-will-need: Chapter 4 'Demand and the Demand Curve', section 4.1 'Quantity Demanded and the Law of Demand'; Chapter 8 'Supply and the Supply Curve', section 8.1 'Quantity Supplied and the Law of Supply'; Chapter 5 'Elasticity of Demand', section 5.1 'Price Elasticity of Demand'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: find new equilibrium after a shift; compute supply elasticity; think: price control below equilibrium; pause: after equilibrium price
sources: no

=== CH10 ===
title: Production Function and Returns
purpose: Shows how inputs become output, why extra workers add less and what happens when all inputs grow.
pace: slow — abstract and numerical
style: calculation
words: 5025
figures: 2
- fig01: total product curve
- fig02: marginal and average product curves
plate: a Victorian workshop with workers added one by one at a single bench
sections:
10.1 | Inputs and Output | new idea | 775
10.2 | The Law of Diminishing Returns | defining theory | 1350
10.3 | Returns to Scale | technique | 300
concepts:
- 10.1 | production function | standard | needs: none
- 10.1 | short run | standard | needs: production function
- 10.1 | long run | standard | needs: short run
- 10.1 | total, average and marginal product | minor | needs: long run
- 10.2 | law of diminishing returns | landmark | needs: total, average and marginal product
- 10.2 | the schedule | minor | needs: law of diminishing returns
- 10.2 | three stages of production | minor | needs: the schedule
- 10.3 | returns to scale | minor | needs: three stages of production
- 10.3 | increasing, constant and decreasing | minor | needs: returns to scale
- 10.3 | causes | minor | needs: increasing, constant and decreasing
defines:
- Production function | A rule showing the largest output that can be made from each combination of inputs.
- Short run | A period so short that at least one input, such as a machine, cannot be changed.
- Long run | A period long enough for all inputs to be changed.
- Total product | The whole quantity of output produced with a given amount of inputs.
- Marginal product | The extra output from adding one more unit of an input, others held fixed.
- Average product | Total product divided by the number of units of the input used.
- Law of diminishing returns | Adding more of one input to fixed amounts of others will, after a point, add less and less to output.
- Returns to scale | The change in output when all inputs are increased in the same proportion.
assumes: Want, Scarcity, Opportunity cost, Supply, Law of supply, Supply schedule
words-in-use: product; scale
case: Binding workshop with one machine: workers 1 to 6 give daily output 10, 24, 36, 44, 48, 48; marginal product 10, 14, 12, 8, 4, 0; average product 10, 12, 12, 11, 9.6, 8. Returns to scale: 2 workers + 1 machine = 24; 4 + 2 = 60; 6 + 3 = 90; 8 + 4 = 108.
history: Thomas Malthus, 1798: An Essay on the Principle of Population argued that food output would rise more slowly than people, a famous use of diminishing returns | search: Malthus 1798 Essay Principle of Population diminishing returns
refs: Chapter 2, 'Scarcity, Choice and Opportunity Cost', section 2.2 'Opportunity Cost' — opportunity cost; Chapter 8, 'Supply and the Supply Curve', section 8.1 'Quantity Supplied and the Law of Supply' — supply
need: you-will-need: Chapter 2 'Scarcity, Choice and Opportunity Cost', section 2.2 'Opportunity Cost'; Chapter 8 'Supply and the Supply Curve', section 8.1 'Quantity Supplied and the Law of Supply'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: complete the product table; classify returns to scale; think: why a seventh worker adds nothing; pause: after the law
sources: no

=== CH11 ===
title: Costs of Production
purpose: Teaches the kinds of cost, how to compute them and how the curves relate.
pace: slow — many linked cost curves
style: calculation
words: 5075
figures: 2
- fig01: fixed, variable and total cost curves
- fig02: marginal, average variable and average total cost curves
plate: a Victorian ledger of a mill with columns for wages, coal and rent
sections:
11.1 | Explicit and Implicit Costs | new idea | 675
11.2 | Short-Run Costs | technique | 1125
11.3 | The Cost Curves | framework | 675
concepts:
- 11.1 | explicit cost | standard | needs: none
- 11.1 | implicit cost | standard | needs: explicit cost
- 11.1 | opportunity cost of own resources | standard | needs: implicit cost
- 11.2 | fixed cost | standard | needs: opportunity cost of own resources
- 11.2 | variable cost | standard | needs: fixed cost
- 11.2 | total cost | standard | needs: variable cost
- 11.2 | marginal cost | standard | needs: total cost
- 11.2 | average costs | standard | needs: marginal cost
- 11.3 | U shape of average cost | standard | needs: average costs
- 11.3 | marginal cost cuts average cost | standard | needs: U shape of average cost
- 11.3 | long-run cost curve | standard | needs: marginal cost cuts average cost
defines:
- Fixed cost | A cost that does not change with the quantity produced in the short run, such as rent.
- Variable cost | A cost that changes with the quantity produced, such as paper and wages for extra output.
- Total cost | Fixed cost plus variable cost at a given output.
- Marginal cost | The extra cost of producing one more unit of output.
- Average total cost | Total cost divided by the number of units produced.
- Explicit cost | A cost that involves a payment of money to outsiders.
- Implicit cost | The value of resources the owner supplies, which involve no payment but have an opportunity cost.
assumes: Production function, Short run, Long run
words-in-use: cost; marginal
case: Binding workshop: fixed cost Tk 600 a day; variable cost 0, 300, 520, 720, 960, 1,300, 1,780 at output 0, 10, 20, 30, 40, 50, 60. Total cost 600, 900, 1,120, 1,320, 1,560, 1,900, 2,380. Marginal cost per unit for each 10 units: 30, 22, 20, 24, 34, 48. Average total cost at 10 to 60: 90, 56, 44, 39, 38, 39.67. Shop 2025: accounting profit 750,000; owner's time valued at 600,000; forgone interest 7% of 4,100,000 = 287,000.
history: Ford Model T, 1908 to 1925: the price fell from about USD 825 to about USD 260 as the assembly line lowered unit cost | search: Model T price 1908 1925
refs: Chapter 10, 'Production Function and Returns', section 10.1 'Inputs and Output' — production function; Chapter 10, 'Production Function and Returns', section 10.2 'The Law of Diminishing Returns' — diminishing returns
need: you-will-need: Chapter 10 'Production Function and Returns', section 10.1 'Inputs and Output'; Chapter 10 'Production Function and Returns', section 10.2 'The Law of Diminishing Returns'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: compute all costs from new data; find output of minimum average cost; think: why marginal cost cuts average cost at its lowest point; pause: after marginal cost
sources: no

=== CH12 ===
title: The Least-Cost Combination of Inputs
purpose: Shows how a firm picks the cheapest mix of inputs to make a given output.
pace: standard — two-input choice
style: calculation
words: 4825
figures: 1
- fig01: isoquant with isocost lines
plate: a Victorian mill with a water wheel beside rows of weaving looms
sections:
12.1 | Choosing Inputs | new idea | 1000
12.2 | The Least-Cost Rule | technique | 675
12.3 | Using the Rule | framework | 550
concepts:
- 12.1 | isoquant | core | needs: none
- 12.1 | isocost line | standard | needs: isoquant
- 12.1 | marginal rate of technical substitution | standard | needs: isocost line
- 12.2 | least-cost combination | standard | needs: marginal rate of technical substitution
- 12.2 | equal marginal product per taka | standard | needs: least-cost combination
- 12.2 | effect of a wage change | standard | needs: equal marginal product per taka
- 12.3 | expansion path | standard | needs: effect of a wage change
- 12.3 | cost-minimising versus profit-maximising choices | standard | needs: expansion path
- 12.3 | when inputs cannot be changed | minor | needs: cost-minimising versus profit-maximising choices
defines:
- Isoquant | A line showing all combinations of two inputs that produce the same output.
- Isocost line | A line showing all combinations of two inputs that cost the same total amount.
- Marginal rate of technical substitution | The amount of one input a firm can give up when it adds one unit of another, keeping output the same.
- Least-cost combination | The mix of inputs that produces a given output at the lowest total cost.
assumes: Production function, Short run, Long run, Fixed cost, Variable cost, Total cost, Indifference curve, Indifference map
words-in-use: input; mix
case: For an output of 100 books bound a day: wage Tk 600 per worker per day; machine rent Tk 400 per machine per day. Combinations (workers, machines) and cost: (2, 10) = 5,200; (3, 6) = 4,200; (4, 4) = 4,000; (5, 3) = 4,200; (6, 2) = 4,400. Least cost is (4, 4) at Tk 4,000; the marginal rate of technical substitution moves from 2 to 1, around the price ratio 600 / 400 = 1.5.
history: Richard Arkwright, Cromford Mill, 1771: water-powered spinning replaced hand labour with machines in a cost-driven change of input mix | search: Arkwright Cromford Mill 1771
refs: Chapter 10, 'Production Function and Returns', section 10.1 'Inputs and Output' — production function; Chapter 11, 'Costs of Production', section 11.1 'Explicit and Implicit Costs' — explicit and implicit costs; Chapter 7, 'Indifference Curve Analysis', section 7.2 'Marginal Rate of Substitution' — marginal rate of substitution
need: you-will-need: Chapter 10 'Production Function and Returns', section 10.1 'Inputs and Output'; Chapter 11 'Costs of Production', section 11.1 'Explicit and Implicit Costs'; Chapter 7 'Indifference Curve Analysis', section 7.2 'Marginal Rate of Substitution'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers, method only; rung 2 realistic messier data; rung 3 changed period, sign or unit; rung 4 combined problem using earlier techniques
practice: review: find the least-cost mix; think: a wage rise; pause: after isocost line
sources: no

=== CH13 ===
title: Market Structure and Perfect Competition
purpose: Introduces the four market structures and studies the firm in perfect competition.
pace: standard — structure of markets
style: mixed
words: 4850
figures: 2
- fig01: firm and industry side by side
- fig02: price taker at marginal cost equals price
plate: a Victorian grain exchange with dealers shouting prices round a central pit
sections:
13.1 | Kinds of Market | new idea | 675
13.2 | The Perfectly Competitive Firm | defining theory | 675
13.3 | Equilibrium of Firm and Industry | framework | 900
concepts:
- 13.1 | market structure | standard | needs: none
- 13.1 | features that distinguish markets | standard | needs: market structure
- 13.1 | industry | standard | needs: features that distinguish markets
- 13.2 | perfect competition | standard | needs: industry
- 13.2 | price taker | standard | needs: perfect competition
- 13.2 | marginal cost equals price | standard | needs: price taker
- 13.3 | equilibrium of the firm | standard | needs: marginal cost equals price
- 13.3 | short-run profit and loss | standard | needs: equilibrium of the firm
- 13.3 | normal profit | standard | needs: short-run profit and loss
- 13.3 | long-run equilibrium | standard | needs: normal profit
defines:
- Market structure | The set of features of a market, such as number of sellers and ease of entry, that shape how firms behave.
- Industry | All the firms that produce the same kind of good.
- Perfect competition | A market with very many buyers and sellers, identical goods, free entry and full information.
- Price taker | A firm that must accept the market price because its own sales are too small to affect it.
- Equilibrium of the firm | The output at which a firm earns the largest possible profit and has no wish to change.
- Normal profit | The least profit a firm must earn to stay in business, covering the owner's implicit costs.
assumes: Elasticity of supply, Competitive market, Equilibrium price, Fixed cost, Variable cost, Total cost
words-in-use: competition; profit
case: Market price Tk 40 (from the notebook market). Cost data of the binding workshop (Chapter 11): marginal cost for each 10-unit step 30, 22, 20, 24, 34, 48 at outputs 10 to 60. Firm produces 50 units: total revenue 50 x 40 = 2,000; total cost 1,900; profit Tk 100.
history: Chicago Board of Trade, 1848: opened as a market for grain, with standard grades and public prices, close to perfect competition | search: Chicago Board of Trade founded 1848
refs: Chapter 9, 'Elasticity of Supply and Market Equilibrium', section 9.2 'Market Equilibrium' — equilibrium price; Chapter 11, 'Costs of Production', section 11.2 'Short-Run Costs' — marginal cost; Chapter 11, 'Costs of Production', section 11.3 'The Cost Curves' — cost curves
need: you-will-need: Chapter 9 'Elasticity of Supply and Market Equilibrium', section 9.2 'Market Equilibrium'; Chapter 11 'Costs of Production', section 11.2 'Short-Run Costs'; Chapter 11 'Costs of Production', section 11.3 'The Cost Curves'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: find the profit-maximising output; think: why profits vanish in the long run; pause: after price taker
sources: no

=== CH14 ===
title: Monopoly and Imperfect Competition
purpose: Studies the single seller and the in-between markets, and how prices are set in each.
pace: standard — other market structures
style: mixed
words: 5050
figures: 2
- fig01: monopolist's demand, marginal revenue and marginal cost
- fig02: four market structures compared
plate: a Victorian diamond merchant weighing stones in a guarded room
sections:
14.1 | Monopoly | defining theory | 900
14.2 | Monopolistic Competition | framework | 675
14.3 | Oligopoly | framework | 675
14.4 | Comparing Markets | framework | 200
concepts:
- 14.1 | monopoly | standard | needs: none
- 14.1 | price maker | standard | needs: monopoly
- 14.1 | marginal revenue | standard | needs: price maker
- 14.1 | price and output of a monopolist | standard | needs: marginal revenue
- 14.2 | monopolistic competition | standard | needs: price and output of a monopolist
- 14.2 | product differentiation | standard | needs: monopolistic competition
- 14.2 | equilibrium of the firm | standard | needs: product differentiation
- 14.3 | oligopoly | standard | needs: equilibrium of the firm
- 14.3 | interdependence | standard | needs: oligopoly
- 14.3 | kinked demand | standard | needs: interdependence
- 14.4 | price in different markets | minor | needs: kinked demand
- 14.4 | is lower price always better | minor | needs: price in different markets
defines:
- Monopoly | A market in which one seller supplies a good with no close substitute.
- Price maker | A firm that can choose its price because it faces the whole demand of the market.
- Marginal revenue | The extra revenue from selling one more unit of output.
- Monopolistic competition | A market with many sellers of similar but not identical goods, and free entry.
- Product differentiation | Making a good different from rivals' goods in quality, style or brand, to build loyal buyers.
- Oligopoly | A market dominated by a few large sellers, each aware of what the others do.
assumes: Market structure, Industry, Perfect competition, Fixed cost, Variable cost, Total cost
words-in-use: monopoly; brand
case: A seller with the whole market for a specialised title: price Tk 70 at 10 units, Tk 60 at 20, Tk 50 at 30, Tk 40 at 40, Tk 30 at 50 (price = 80 - quantity); total revenue 700, 1,200, 1,500, 1,600, 1,500; marginal revenue per step 70, 50, 30, 10, -10; marginal cost per step 30, 22, 20, 24, 34. Output 30 at price Tk 50; total cost 1,320; profit Tk 180.
history: De Beers, 1888: Cecil Rhodes merged South African diamond mines into a company that controlled most of the world's supply | search: De Beers Consolidated Mines 1888 Rhodes
refs: Chapter 13, 'Market Structure and Perfect Competition', section 13.1 'Kinds of Market' — market structure; Chapter 13, 'Market Structure and Perfect Competition', section 13.2 'The Perfectly Competitive Firm' — price taker; Chapter 11, 'Costs of Production', section 11.2 'Short-Run Costs' — marginal cost
need: you-will-need: Chapter 13 'Market Structure and Perfect Competition', section 13.1 'Kinds of Market'; Chapter 13 'Market Structure and Perfect Competition', section 13.2 'The Perfectly Competitive Firm'; Chapter 11 'Costs of Production', section 11.2 'Short-Run Costs'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: find monopoly output; compare with competition; think: whether lower prices are always better; pause: after marginal revenue
sources: no

=== CH15 ===
title: Rent and Wages
purpose: Explains why land earns rent and why wages differ and how they are set.
pace: standard — payments to land and labour
style: mixed
words: 4850
figures: 2
- fig01: differential rent as stacked grades of land
- fig02: wage set by labour demand and supply
plate: a Victorian hiring fair with workers waiting beside farm carts
sections:
15.1 | Rent | new idea | 900
15.2 | Wages | framework | 675
15.3 | How Wages Are Set | framework | 675
concepts:
- 15.1 | rent | standard | needs: none
- 15.1 | economic rent | standard | needs: rent
- 15.1 | Ricardian theory of rent | standard | needs: economic rent
- 15.1 | quasi-rent | standard | needs: Ricardian theory of rent
- 15.2 | wage | standard | needs: quasi-rent
- 15.2 | nominal wage | standard | needs: wage
- 15.2 | real wage | standard | needs: nominal wage
- 15.3 | marginal revenue product | standard | needs: real wage
- 15.3 | demand for and supply of labour | standard | needs: marginal revenue product
- 15.3 | why wages differ | standard | needs: demand for and supply of labour
defines:
- Rent | The payment to the owner of land for its use, in everyday talk and in economics.
- Economic rent | The payment to any factor above the least needed to keep it in use.
- Quasi-rent | A temporary surplus earned by a man-made asset while its supply is fixed in the short run.
- Wage | The payment to labour for its work over a period.
- Nominal wage | A wage measured in money, without allowing for changes in prices.
- Real wage | A wage measured by the goods and services it can buy.
- Marginal revenue product | The extra revenue a firm gains from employing one more unit of labour.
assumes: Monopoly, Price maker, Marginal revenue, Elasticity of supply, Competitive market, Equilibrium price, Fixed cost, Variable cost
words-in-use: rent; wage
case: Differential rent: shop profit before rent at Dhanmondi Tk 1,470,000; at the marginal site that pays no rent Tk 750,000; difference Tk 720,000 equals the rent paid (Tk 60,000 x 12). Wages: nominal Tk 35,000 a month; price index from 100 to 112; real wage = 35,000 / 1.12 = Tk 31,250. Extra assistants add Tk 40,000 and Tk 30,000 a month of revenue; wage Tk 35,000.
history: David Ricardo, 1817: On the Principles of Political Economy set out the theory of rent; Bangladesh raised the garment minimum wage in 2013 | search: Ricardo rent 1817; Bangladesh garment minimum wage 2013 Tk 5,300
refs: Chapter 14, 'Monopoly and Imperfect Competition', section 14.1 'Monopoly' — monopoly; Chapter 9, 'Elasticity of Supply and Market Equilibrium', section 9.3 'Changes in Equilibrium' — changes in equilibrium; Chapter 11, 'Costs of Production', section 11.1 'Explicit and Implicit Costs' — explicit and implicit costs
need: you-will-need: Chapter 14 'Monopoly and Imperfect Competition', section 14.1 'Monopoly'; Chapter 9 'Elasticity of Supply and Market Equilibrium', section 9.3 'Changes in Equilibrium'; Chapter 11 'Costs of Production', section 11.1 'Explicit and Implicit Costs'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: compute differential rent; compute real wage; think: why surgeons earn more; pause: after Ricardian rent
sources: no

=== CH16 ===
title: Interest and Profit
purpose: Explains what sets the rate of interest and why profit arises.
pace: standard — payments to capital and enterprise
style: mixed
words: 4850
figures: 1
- fig01: loanable funds demand and supply
plate: a Victorian bank manager and a farmer across a desk with a bond and an inkwell
sections:
16.1 | Interest | new idea | 900
16.2 | Profit | framework | 675
16.3 | Why Profit Occurs | framework | 675
concepts:
- 16.1 | interest | standard | needs: none
- 16.1 | loanable funds theory | standard | needs: interest
- 16.1 | liquidity preference | standard | needs: loanable funds theory
- 16.1 | rates for different loans | standard | needs: liquidity preference
- 16.2 | accounting profit | standard | needs: rates for different loans
- 16.2 | economic profit | standard | needs: accounting profit
- 16.2 | normal and supernormal profit | standard | needs: economic profit
- 16.3 | risk bearing theory | standard | needs: normal and supernormal profit
- 16.3 | marginal productivity view | standard | needs: risk bearing theory
- 16.3 | innovation and monopoly | standard | needs: marginal productivity view
defines:
- Interest | The payment a borrower makes to a lender for the use of money over time.
- Loanable funds | The money that savers offer and borrowers demand at each interest rate.
- Liquidity preference | The wish of people to hold money in cash form rather than lend it.
- Accounting profit | Revenue minus explicit costs only.
- Economic profit | Revenue minus all costs, explicit and implicit, including normal profit.
- Risk bearing | The act of taking on the chance of loss in business, which profit rewards.
assumes: Rent, Economic rent, Quasi-rent, Fixed cost, Variable cost, Total cost, Market structure, Industry
words-in-use: interest; profit
case: Bank loan Tk 600,000 at 11% a year: interest Tk 66,000. Shop 2025: accounting profit Tk 750,000; implicit costs Tk 600,000 (owner's time) + Tk 287,000 (forgone interest on Tk 4,100,000 at 7%) = Tk 887,000; economic profit = 750,000 - 887,000 = -Tk 137,000.
history: Muhammad Yunus, 1976: began lending small sums in Jobra village, leading to the Grameen Bank in 1983 and a 2006 Nobel Peace Prize | search: Yunus Jobra 1976 Grameen Bank 1983
refs: Chapter 15, 'Rent and Wages', section 15.1 'Rent' — rent; Chapter 11, 'Costs of Production', section 11.1 'Explicit and Implicit Costs' — explicit and implicit costs; Chapter 13, 'Market Structure and Perfect Competition', section 13.3 'Equilibrium of Firm and Industry' — normal profit
need: you-will-need: Chapter 15 'Rent and Wages', section 15.1 'Rent'; Chapter 11 'Costs of Production', section 11.1 'Explicit and Implicit Costs'; Chapter 13 'Market Structure and Perfect Competition', section 13.3 'Equilibrium of Firm and Industry'; Appendix A1 'Maths You Will Use'
ladder: rung 1 clean small numbers or a familiar case; rung 2 messier realistic data or a reshaped case; rung 3 a changed unit or a new setting; rung 4 a combined problem using earlier ideas
practice: review: compute economic profit; think: why a risky venture earns more; pause: after economic profit
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
