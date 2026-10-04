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
course: MGT-102 Principles of Management
book: Principles of Management
spelling: British · dates: day, month, year (12 March 2026) · country: mixed, no single country; the running case is set in Bangladesh, examples and history draw on several countries, and the place is named whenever law or standards differ · currency and numbers: taka for the running case, written Tk 158,000 (international digit grouping); a historical case keeps its own currency
running case: Meghna and Sons — illustrative documented case file. Base facts (FIXED): Meghna and Sons, a family-run bookshop in Dhanmondi, Dhaka, founded 4 April 1996 by Nurul Meghna (owner-manager). Staff 7 in 2025: Rafiq Meghna (store manager), Tariq Meghna (accounts and online orders), 3 sales assistants, 1 cashier, 1 stock clerk. 2025: sales Tk 14,400,000; copies sold 41,000; stock at cost Tk 3,500,000; purchases Tk 9,800,000; profit Tk 750,000. Sales by customer 2025: walk-in Tk 9,000,000; schools Tk 3,600,000; online Tk 1,800,000.
chosen words (one term, one word):
- use manager — also called executive, administrator (only when speaking of the person)
- use organisation — also called institution, enterprise (one word: organisation)
- use employee — also called worker, subordinate, staff member
- use goal — also called aim, target (use objective for the formal term; goal only in plain talk)
- use authority — also called right to command (power is a separate term)
size plan: 18 chapters, ~93400 words, ~361 pages (target 360)
chapters:
1. What Management Is
2. Managers and Their Skills
3. The Functions of Management and How Management Is Judged
4. The Evolution of Management Thought
5. The Environment of Organisations
6. Social Responsibility, Ethics and Sustainability
7. Planning: Meaning, Nature and Steps
8. Kinds of Plans and Making Planning Work
9. Objectives and Management by Objectives
10. Decision Making: Process and Conditions
11. Decision Tools and Group Decisions
12. Organizing and Types of Organization
13. Structure: Span, Departments and Forces
14. Authority, Delegation and Coordination
15. Human Factors, Groups and Creativity
16. Motivation
17. Leadership
18. Controlling

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
title: What Management Is
purpose: Builds a plain picture of what management is, why it exists and what a manager answers for.
pace: slow — first chapter; introduces the central word and teaches how to read the book
style: descriptive
words: 5250
figures: 1
- fig01: managers turning resources and goals into results
plate: a Victorian mill manager's office with a window over the factory floor and a desk of reports
sections:
1.1 | The Idea of Management | new idea | 900
1.2 | Nature of Management | framework | 775
1.3 | Purpose and Importance | framework | 675
1.4 | A Manager's Responsibility | framework | 300
concepts:
- 1.1 | management | standard | needs: none
- 1.1 | organisation | standard | needs: management
- 1.1 | resources | standard | needs: organisation
- 1.1 | results through people | standard | needs: resources
- 1.2 | nature of management | standard | needs: results through people
- 1.2 | universal and continuing | standard | needs: nature of management
- 1.2 | group activity | standard | needs: universal and continuing
- 1.2 | goal-directed | minor | needs: group activity
- 1.3 | purpose of management | standard | needs: goal-directed
- 1.3 | why organisations need it | standard | needs: purpose of management
- 1.3 | importance | standard | needs: why organisations need it
- 1.4 | managerial responsibility | minor | needs: importance
- 1.4 | answering for results | minor | needs: managerial responsibility
- 1.4 | to owners, staff and customers | minor | needs: answering for results
defines:
- Management | The work of planning, organising, leading and controlling people and resources to reach an organisation's goals.
- Organisation | A group of people working together in a planned way to reach goals they could not reach alone.
- Manager | A person who is responsible for the work of other people and for the results of their work.
- Resource | Anything an organisation uses to reach its goals, such as people, money, materials and information.
- Responsibility | A duty to carry out a task and to answer for how well it is done.
assumes: none
words-in-use: management; resource
case: Position 1 January 2026: owner-manager Nurul Meghna; 7 staff (Rafiq store manager, Tariq accounts and online orders, 3 sales assistants, 1 cashier, 1 stock clerk); resources: stock Tk 3,500,000, fittings Tk 600,000, 7 staff. 2026 sales target Tk 15,840,000 (10% above Tk 14,400,000).
history: Robert Owen, 1800: took over the New Lanark cotton mills in Scotland and managed workers' hours, housing and schooling alongside output | search: Robert Owen New Lanark 1800
refs: none
need: nothing
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: name the resources a hospital uses; separate responsibility from authority; think: why a one-person stall still needs management; pause: after the definition of management
sources: no

=== CH02 ===
title: Managers and Their Skills
purpose: Shows the kinds of manager at each level and the skills each level needs.
pace: slow — second chapter; many new labels for roles and abilities
style: descriptive
words: 5275
figures: 1
- fig01: three levels of management with the skills each needs
plate: a Victorian railway company boardroom with directors, a station master and a foreman standing apart
sections:
2.1 | Levels and Kinds of Manager | new idea | 900
2.2 | Skills Managers Need | framework | 1225
2.3 | Managers and Resources | framework | 550
concepts:
- 2.1 | top manager | standard | needs: none
- 2.1 | middle manager | standard | needs: top manager
- 2.1 | first-line manager | standard | needs: middle manager
- 2.1 | functional and general managers | standard | needs: first-line manager
- 2.2 | technical skill | standard | needs: functional and general managers
- 2.2 | human skill | standard | needs: technical skill
- 2.2 | conceptual skill | standard | needs: human skill
- 2.2 | skill mix by level | core | needs: conceptual skill
- 2.3 | types of resource managed | standard | needs: skill mix by level
- 2.3 | the manager's roles | standard | needs: types of resource managed
- 2.3 | qualities of an effective manager | minor | needs: the manager's roles
defines:
- Top manager | A manager at the highest level, who sets the direction of the whole organisation.
- Middle manager | A manager between top and first-line managers, who turns the direction into plans for departments.
- First-line manager | A manager who directly supervises employees doing the day-to-day work.
- Technical skill | The ability to use the tools, methods and knowledge of a specific job.
- Human skill | The ability to work with, understand and motivate other people.
- Conceptual skill | The ability to see the organisation as a whole and to understand how its parts connect.
assumes: Management, Organisation, Manager
words-in-use: role; level
case: Roles 2026: top = Nurul (sets prices, hires, approves loans); middle = Rafiq (weekly stock plan, supervises 5 staff) and Tariq (accounts, online orders); first-line work = Rafiq's direct supervision of 3 sales assistants, 1 cashier, 1 stock clerk. Time use of Nurul in a typical week of 48 hours: 20 hours planning and dealing with publishers, 16 hours on the shop floor, 12 hours on staff and money matters.
history: Robert Katz, 1955: Harvard Business Review article on the skills of an effective administrator, naming technical, human and conceptual skill | search: Katz 1955 Skills of an Effective Administrator
refs: Chapter 1, 'What Management Is', section 1.1 'The Idea of Management' — the manager defined; Chapter 1, 'What Management Is', section 1.4 'A Manager's Responsibility' — responsibility
need: you-will-need: Chapter 1 'What Management Is', section 1.1 'The Idea of Management'; Chapter 1 'What Management Is', section 1.4 'A Manager's Responsibility'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: place named jobs at a level; think: which skill a new supervisor lacks; pause: after the three skills
sources: no

=== CH03 ===
title: The Functions of Management and How Management Is Judged
purpose: Teaches the four functions, the measures of performance, and whether management is a science, an art or a profession.
pace: slow — second tier of ideas that must not blur
style: descriptive
words: 5300
figures: 2
- fig01: functions as a cycle with the three measures beside it
- fig02: three claims about management compared by what each demands
plate: a Victorian engineering drawing office with a long draughtsman's table and wall plans
sections:
3.1 | The Management Process | new idea | 1125
3.2 | Productivity | new idea | 675
3.3 | Effectiveness and Efficiency | framework | 300
3.4 | Science, Art or Profession | framework | 400
3.5 | Importance and Principles in Brief | framework | 200
concepts:
- 3.1 | management process | standard | needs: none
- 3.1 | planning | standard | needs: management process
- 3.1 | organising | standard | needs: planning
- 3.1 | leading | standard | needs: organising
- 3.1 | controlling | standard | needs: leading
- 3.2 | productivity | standard | needs: controlling
- 3.2 | measuring output per input | standard | needs: productivity
- 3.2 | raising productivity | standard | needs: measuring output per input
- 3.3 | effectiveness | minor | needs: raising productivity
- 3.3 | efficiency | minor | needs: effectiveness
- 3.3 | telling them apart | minor | needs: efficiency
- 3.4 | management as a science | minor | needs: telling them apart
- 3.4 | management as an art | minor | needs: management as a science
- 3.4 | management as a profession | minor | needs: management as an art
- 3.4 | tests of a profession | minor | needs: management as a profession
- 3.5 | importance of management | minor | needs: tests of a profession
- 3.5 | principles of management in brief | minor | needs: importance of management
defines:
- Management process | The continuing cycle of planning, organising, leading and controlling that managers repeat.
- Productivity | The amount of output an organisation obtains for each unit of input it uses.
- Effectiveness | How far an organisation reaches its goals, whatever the cost.
- Efficiency | How little input an organisation uses to reach a given result.
- Profession | An occupation that requires long training, a body of knowledge and a code of conduct enforced by an association.
- Principle of management | A broad guide to action drawn from experience, to be adapted and not applied like a law.
- Function of management | One of the four main kinds of work managers do: planning, organising, leading or controlling.
assumes: Management, Organisation, Manager, Top manager, Middle manager, First-line manager
words-in-use: process; art; science
case: Choices in 2026: stock reordering follows a measured rule (reorder when 30 days of cover remain); shop-window design for the textbook season depends on Rafiq's judgement; no licence or governing body is required to manage the shop. Productivity record: 2025 copies sold 41,000 with 7 staff = 5,857 copies per employee; 2026 forecast 45,100 copies with 7 staff = 6,443 per employee (rounded). Effectiveness check: 2026 sales target Tk 15,840,000; efficiency: 3 hours a day spent searching for stock cut to 1 hour after relabelling shelves on 2 March 2026.
history: Toyota Production System, 1940s to 1970s: Taiichi Ohno built a system aimed at removing waste, a landmark in efficiency | search: Taiichi Ohno Toyota Production System history
refs: Chapter 1, 'What Management Is', section 1.2 'Nature of Management' — nature of management; Chapter 2, 'Managers and Their Skills', section 2.1 'Levels and Kinds of Manager' — levels
need: you-will-need: Chapter 1 'What Management Is', section 1.2 'Nature of Management'; Chapter 2 'Managers and Their Skills', section 2.1 'Levels and Kinds of Manager'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: calculate productivity for a bakery with new data; think: effective but not efficient; pause: after effectiveness
sources: no

=== CH04 ===
title: The Evolution of Management Thought
purpose: Traces the main schools of management thought and sets out Fayol's fourteen principles.
pace: standard — history of ideas that explain later chapters
style: descriptive
words: 5225
figures: 2
- fig01: timeline of schools of thought
- fig02: Fayol's fourteen principles in groups
plate: a Victorian factory floor with a time-keeper at a clock and rows of workers at benches
sections:
4.1 | Early Management and Scientific Management | defining theory | 675
4.2 | Administrative Principles | defining theory | 1250
4.3 | Bureaucratic and Behavioural Thought | framework | 300
4.4 | Modern Approaches | framework | 400
concepts:
- 4.1 | scientific management | standard | needs: none
- 4.1 | time and motion study | standard | needs: scientific management
- 4.1 | Taylor's four principles | standard | needs: time and motion study
- 4.2 | Fayol's general principles | landmark | needs: Taylor's four principles
- 4.2 | the fourteen principles | minor | needs: Fayol's general principles
- 4.3 | bureaucratic model | minor | needs: the fourteen principles
- 4.3 | human relations school | minor | needs: bureaucratic model
- 4.3 | behavioural science | minor | needs: human relations school
- 4.4 | management science approach | minor | needs: behavioural science
- 4.4 | systems approach | minor | needs: management science approach
- 4.4 | contingency approach | minor | needs: systems approach
- 4.4 | comparing approaches | minor | needs: contingency approach
defines:
- Scientific management | The study of work by measurement, aimed at finding the one best method and training workers in it.
- Time and motion study | Timing each movement of a task to remove wasted effort and set a fair standard.
- Bureaucracy | A system of management built on written rules, clear levels of authority and appointment by skill.
- Human relations school | A school of thought holding that workers' feelings and social bonds strongly affect output.
- Contingency approach | The view that the best way to manage depends on the situation.
- Management science approach | The use of mathematical models and data to solve management problems.
assumes: Management process, Productivity, Effectiveness
words-in-use: school; law
case: Method change 1 March 2026: unloading a carton of books timed at 14 minutes before and 9 minutes after a fixed shelf-position list; 10 cartons a day; saving 5 x 10 = 50 minutes a day. Rule list posted: 14 rules for the shop.
history: Hawthorne studies, 1924 to 1932: experiments at Western Electric near Chicago showed that attention and group bonds affected output | search: Hawthorne studies Western Electric 1924
refs: Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.4 'Science, Art or Profession' — scientific claim; Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — management process
need: you-will-need: Chapter 3 'The Functions of Management and How Management Is Judged', section 3.4 'Science, Art or Profession'; Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: match thinkers to ideas; think: which school suits a busy kitchen; pause: after Fayol
sources: no

=== CH05 ===
title: The Environment of Organisations
purpose: Shows the forces around an organisation and how managers study and shape them.
pace: standard — many outside forces
style: descriptive
words: 5175
figures: 1
- fig01: external environment layers around the organisation
plate: a Victorian port with customs sheds, ships and a clerk watching the weather vane
sections:
5.1 | Inside and Outside the Organisation | new idea | 550
5.2 | Components of the External Environment | framework | 1125
5.3 | Managing the Environment | framework | 900
concepts:
- 5.1 | internal environment | standard | needs: none
- 5.1 | external environment | standard | needs: internal environment
- 5.1 | boundary | minor | needs: external environment
- 5.2 | economic environment | standard | needs: boundary
- 5.2 | social and cultural environment | standard | needs: economic environment
- 5.2 | political and legal environment | standard | needs: social and cultural environment
- 5.2 | technological environment | standard | needs: political and legal environment
- 5.2 | global environment | standard | needs: technological environment
- 5.3 | scanning | standard | needs: global environment
- 5.3 | adapting | standard | needs: scanning
- 5.3 | influencing | standard | needs: adapting
- 5.3 | strategic response | standard | needs: influencing
defines:
- Internal environment | Conditions inside an organisation, such as its people, structure and culture, which managers can change directly.
- External environment | All outside forces, such as customers, rivals, laws and technology, that affect an organisation.
- Environmental scanning | Regularly collecting and studying information about outside changes that could affect the organisation.
- Task environment | The outside groups that deal directly with an organisation, such as customers and suppliers.
assumes: Management process, Productivity, Effectiveness, Management, Organisation, Manager
words-in-use: environment; climate
case: Outside changes recorded: online price cuts of about 15% on textbooks since March 2025; textbook sales Tk 4,300,000 in 2025 against Tk 5,100,000 in 2024; curriculum change from January 2026; response of 3 March 2026: price-match list for 200 titles. Inside: 2 of 7 staff left in 2025.
history: Nokia, 2007 to 2013: lost its lead in mobile phones after the smartphone shift and sold its devices unit to Microsoft in 2013 | search: Nokia Microsoft devices sale 2013
refs: Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — management process; Chapter 1, 'What Management Is', section 1.1 'The Idea of Management' — organisation
need: you-will-need: Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'; Chapter 1 'What Management Is', section 1.1 'The Idea of Management'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: sort forces; think: a law changes; pause: after external environment
sources: no

=== CH06 ===
title: Social Responsibility, Ethics and Sustainability
purpose: Explains what organisations owe society, how ethics guides choices and what sustainability asks.
pace: standard — moral questions and a model
style: descriptive
words: 5275
figures: 1
- fig01: Davis model as steps from power to responsibility
plate: a Victorian public meeting hall with a speaker on a platform and a crowd of working people
sections:
6.1 | Social Responsibility | new idea | 1000
6.2 | Business Ethics | framework | 1000
6.3 | Sustainability | framework | 675
concepts:
- 6.1 | corporate social responsibility | core | needs: none
- 6.1 | arguments for and against | standard | needs: corporate social responsibility
- 6.1 | Davis model | standard | needs: arguments for and against
- 6.2 | business ethics | core | needs: Davis model
- 6.2 | ethical dilemma | standard | needs: business ethics
- 6.2 | ways to promote ethics | standard | needs: ethical dilemma
- 6.3 | sustainability | standard | needs: ways to promote ethics
- 6.3 | triple bottom line | standard | needs: sustainability
- 6.3 | recognising a responsible organisation | standard | needs: triple bottom line
defines:
- Corporate social responsibility | An organisation's duty to act for the good of society as well as for profit.
- Business ethics | The moral rules that decide what is right and wrong in business conduct.
- Ethical dilemma | A choice in which every option has a cost to some person's rights or interests.
- Triple bottom line | A way of judging an organisation by its profit, its effect on people and its effect on the natural environment.
- Sustainability | Running an organisation so that it meets today's needs without harming later generations' ability to meet theirs.
assumes: Internal environment, External environment, Environmental scanning, Management, Organisation, Manager
words-in-use: responsibility; ethics
case: Ethical choice, 18 June 2026: a supplier offered 500 photocopied copies of a textbook at Tk 180 each against Tk 260 for genuine copies, a saving of 500 x 80 = Tk 40,000; the shop declined; 380 kg of unsold paper sent to recycling in 2025.
history: Rana Plaza, 24 April 2013: a garment building collapsed at Savar, Bangladesh, killing more than 1,100 people; the Accord on Fire and Building Safety followed in May 2013 | search: Rana Plaza 2013 Accord on Fire and Building Safety
refs: Chapter 5, 'The Environment of Organisations', section 5.1 'Inside and Outside the Organisation' — external forces from society; Chapter 1, 'What Management Is', section 1.4 'A Manager's Responsibility' — responsibility
need: you-will-need: Chapter 5 'The Environment of Organisations', section 5.1 'Inside and Outside the Organisation'; Chapter 1 'What Management Is', section 1.4 'A Manager's Responsibility'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: sort an act as legal, ethical or both; think: a rival cuts safety to cut price; pause: after ethical dilemma
sources: no

=== CH07 ===
title: Planning: Meaning, Nature and Steps
purpose: Teaches what planning is and the steps that turn a goal into a plan.
pace: slow — planning is new and abstract
style: descriptive
words: 5175
figures: 1
- fig01: planning steps as a ladder
plate: a Victorian architect at a drawing board with plans, compasses and a model bridge
sections:
7.1 | What Planning Is | new idea | 1000
7.2 | Steps in Planning | technique | 1125
7.3 | Why Plans Fail | framework | 450
concepts:
- 7.1 | planning | core | needs: none
- 7.1 | nature of planning | standard | needs: planning
- 7.1 | purposes of planning | standard | needs: nature of planning
- 7.2 | awareness of opportunity | standard | needs: purposes of planning
- 7.2 | setting objectives | standard | needs: awareness of opportunity
- 7.2 | planning premises | standard | needs: setting objectives
- 7.2 | listing and weighing alternatives | standard | needs: planning premises
- 7.2 | choosing and carrying out | standard | needs: listing and weighing alternatives
- 7.3 | reasons plans fail | standard | needs: choosing and carrying out
- 7.3 | limits to planning | standard | needs: reasons plans fail
defines:
- Plan | A written or spoken statement of what will be done, by whom and by when, to reach an objective.
- Planning premise | An assumption about future conditions on which a plan is built.
- Alternative | One of several possible courses of action from which a choice is made.
- Forecast | A reasoned estimate of what will happen in the future, used as a base for a plan.
assumes: Management process, Productivity, Effectiveness
words-in-use: plan; premise
case: Planning for the textbook season: season starts 2 January 2026; order deadline 12 December 2025; expected 1,800 textbook sets at an average Tk 2,400 = Tk 4,320,000; premise: publishers raise prices by 5%; supplier lead time 21 days.
history: Apollo programme: on 25 May 1961 President Kennedy set a goal of landing a person on the Moon; Apollo 11 landed on 20 July 1969 | search: Kennedy 25 May 1961 Apollo 11 landing
refs: Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — the planning function; Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.2 'Productivity' — measures of performance
need: you-will-need: Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'; Chapter 3 'The Functions of Management and How Management Is Judged', section 3.2 'Productivity'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: order the steps; think: a plan built on a wrong premise; pause: after planning premise
sources: no

=== CH08 ===
title: Kinds of Plans and Making Planning Work
purpose: Distinguishes standing plans from single-use plans and shows how to make plans work.
pace: standard — classification
style: descriptive
words: 5075
figures: 1
- fig01: plans arranged by time span and by repeat use
plate: a Victorian army staff officer's table with maps and pins before a landing
sections:
8.1 | Kinds of Plan | framework | 1125
8.2 | Time Span and Level | framework | 450
8.3 | Making Planning Effective | framework | 900
concepts:
- 8.1 | standing plan | standard | needs: none
- 8.1 | single-use plan | standard | needs: standing plan
- 8.1 | policy, procedure and rule | standard | needs: single-use plan
- 8.1 | programme and project | standard | needs: policy, procedure and rule
- 8.1 | budget | standard | needs: programme and project
- 8.2 | long-range and short-range plans | standard | needs: budget
- 8.2 | strategic and operational plans | standard | needs: long-range and short-range plans
- 8.3 | support from top | standard | needs: strategic and operational plans
- 8.3 | clear objectives | standard | needs: support from top
- 8.3 | communication and flexibility | standard | needs: clear objectives
- 8.3 | overcoming limits | standard | needs: communication and flexibility
defines:
- Standing plan | A plan made once and used again and again for situations that repeat.
- Single-use plan | A plan made for one occasion or project and not used again.
- Policy | A general guide for decisions, setting limits within which choices are made.
- Procedure | A fixed series of steps to be followed for a repeated task.
- Budget | A plan that states expected income and spending in money terms for a period.
assumes: Plan, Planning premise, Alternative, Management process, Productivity, Effectiveness
words-in-use: rule; programme
case: Plans 2026: standing: returns policy (exchange within 7 days with a receipt); single-use: a 28-day book fair stall in February 2026 with budget Tk 240,000 and expected sales Tk 900,000; yearly budget: sales Tk 15,840,000.
history: Operation Overlord, 1944: the planned Allied landings in Normandy on 6 June 1944 were a single-use plan on a very large scale | search: Operation Overlord planning 1944
refs: Chapter 7, 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is' — planning basics; Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — management process
need: you-will-need: Chapter 7 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is'; Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: sort plans; think: when a standing plan should be changed; pause: after single-use plan
sources: no

=== CH09 ===
title: Objectives and Management by Objectives
purpose: Explains objectives and the system that manages by them.
pace: standard — new and abstract
style: descriptive
words: 5275
figures: 1
- fig01: the MBO cycle as a ring
plate: a Victorian signpost at a crossroads with distances and a lone traveller
sections:
9.1 | Objectives | new idea | 675
9.2 | Setting Objectives | technique | 550
9.3 | Management by Objectives | defining theory | 1450
concepts:
- 9.1 | objective | standard | needs: none
- 9.1 | nature of objectives | standard | needs: objective
- 9.1 | key result areas | standard | needs: nature of objectives
- 9.2 | setting objectives | standard | needs: key result areas
- 9.2 | measurable aim | standard | needs: setting objectives
- 9.2 | linking levels | minor | needs: measurable aim
- 9.3 | management by objectives | landmark | needs: linking levels
- 9.3 | process of MBO | minor | needs: management by objectives
- 9.3 | benefits of MBO | minor | needs: process of MBO
- 9.3 | weaknesses of MBO | minor | needs: benefits of MBO
defines:
- Objective | A specific result an organisation or person intends to reach within a stated time.
- Management by objectives | A system in which managers and employees agree objectives together, then judge performance against them.
- Key result area | A part of the organisation's work where results are vital to success.
- Performance appraisal | A review of how well an employee has done against agreed standards.
assumes: Plan, Planning premise, Alternative, Standing plan, Single-use plan, Policy
words-in-use: objective; target
case: 2026 objectives: sales Tk 15,840,000; profit Tk 900,000; returns below 2% of sales; 14 hours of customer-service training per employee; stock turnover from 2.8 to 3.2 times (cost of sales Tk 9,800,000 / average stock Tk 3,500,000 = 2.8; at 3.2, stock Tk 3,062,500). Agreed between Nurul and Rafiq on 5 January 2026.
history: Peter Drucker, 1954: The Practice of Management described management by objectives | search: Drucker 1954 Practice of Management MBO
refs: Chapter 7, 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is' — planning; Chapter 8, 'Kinds of Plans and Making Planning Work', section 8.1 'Kinds of Plan' — kinds of plan
need: you-will-need: Chapter 7 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is'; Chapter 8 'Kinds of Plans and Making Planning Work', section 8.1 'Kinds of Plan'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: write measurable objectives; think: MBO in a school; pause: after process of MBO
sources: no

=== CH10 ===
title: Decision Making: Process and Conditions
purpose: Explains how managers decide and what conditions surround a decision.
pace: slow — abstract and layered
style: descriptive
words: 5075
figures: 1
- fig01: decision steps as a flow
plate: a Victorian magistrate's bench with scales, books and a clerk
sections:
10.1 | What a Decision Is | new idea | 675
10.2 | The Decision Process | technique | 1125
10.3 | Conditions of Decision Making | framework | 675
concepts:
- 10.1 | decision making | standard | needs: none
- 10.1 | problem and opportunity | standard | needs: decision making
- 10.1 | nature of managerial decision making | standard | needs: problem and opportunity
- 10.2 | decision criteria | standard | needs: nature of managerial decision making
- 10.2 | developing alternatives | standard | needs: decision criteria
- 10.2 | evaluating | standard | needs: developing alternatives
- 10.2 | choosing | standard | needs: evaluating
- 10.2 | implementing and following up | standard | needs: choosing
- 10.3 | certainty | standard | needs: implementing and following up
- 10.3 | risk | standard | needs: certainty
- 10.3 | uncertainty | standard | needs: risk
defines:
- Decision making | Choosing one course of action from several, in order to solve a problem or use an opportunity.
- Certainty | A condition in which the result of each alternative is known in advance.
- Risk | A condition in which the result is not known but its chances can be estimated.
- Uncertainty | A condition in which neither the results nor their chances can be estimated.
- Decision criterion | A standard used to judge which alternative is best.
assumes: Plan, Planning premise, Alternative, Objective, Management by objectives, Key result area
words-in-use: risk; decision
case: Decision, 20 August 2026: open a second shop? Options: Mirpur (rent Tk 38,000 a month; fitting Tk 650,000; expected sales Tk 5,400,000 a year); Uttara (rent Tk 52,000 a month; fitting Tk 720,000; expected sales Tk 5,100,000 a year); wait a year.
history: Challenger, 27 to 28 January 1986: the launch decision went ahead despite engineers' warnings about cold-weather seals | search: Challenger launch decision Rogers Commission 1986
refs: Chapter 7, 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is' — planning; Chapter 9, 'Objectives and Management by Objectives', section 9.2 'Setting Objectives' — setting objectives
need: you-will-need: Chapter 7 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is'; Chapter 9 'Objectives and Management by Objectives', section 9.2 'Setting Objectives'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: classify decisions by condition; think: a decision with missing data; pause: after the process
sources: no

=== CH11 ===
title: Decision Tools and Group Decisions
purpose: Shows the tools and group methods managers use to improve decisions.
pace: standard — tools with numbers but descriptive style
style: descriptive
words: 5050
figures: 1
- fig01: a simple decision tree drawn as branches
plate: a Victorian committee table with members, papers and an inkwell
sections:
11.1 | Probability and Decision Trees | technique | 1325
11.2 | Decision Support Systems | framework | 450
11.3 | Deciding in Groups | framework | 675
concepts:
- 11.1 | probability | standard | needs: none
- 11.1 | expected value | core | needs: probability
- 11.1 | decision tree | core | needs: expected value
- 11.2 | decision support system | standard | needs: decision tree
- 11.2 | information for decisions | standard | needs: decision support system
- 11.3 | group decision | standard | needs: information for decisions
- 11.3 | advantages and disadvantages | standard | needs: group decision
- 11.3 | other factors in decisions | standard | needs: advantages and disadvantages
defines:
- Expected value | The average result of a choice, found by weighting each possible result by its chance.
- Decision tree | A diagram of choices and chance events, drawn as branches, showing the value of each path.
- Decision support system | A computer-based tool that helps managers study data and compare alternatives.
- Group decision | A choice reached by several people discussing and agreeing, instead of one person alone.
assumes: Decision making, Certainty, Risk
words-in-use: value; tree
case: Tree for the second shop: success chance 0.6 with profit Tk 900,000; failure chance 0.4 with loss Tk 300,000; expected value = 0.6 x 900,000 + 0.4 x (-300,000) = 540,000 - 120,000 = Tk 420,000; waiting = Tk 0. Group of 3 (Nurul, Rafiq, Tariq) took 3 days; Nurul alone 1 day. Sales software lists the top 20 titles each week.
history: Herbert Simon, 1947: Administrative Behavior described satisficing; he won the 1978 economics Nobel prize | search: Herbert Simon satisficing Administrative Behavior 1947
refs: Chapter 10, 'Decision Making: Process and Conditions', section 10.3 'Conditions of Decision Making' — conditions of decision; Chapter 10, 'Decision Making: Process and Conditions', section 10.2 'The Decision Process' — process
need: you-will-need: Chapter 10 'Decision Making: Process and Conditions', section 10.3 'Conditions of Decision Making'; Chapter 10 'Decision Making: Process and Conditions', section 10.2 'The Decision Process'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: compute expected value for new data; think: when groups slow decisions; pause: after expected value
sources: no

=== CH12 ===
title: Organizing and Types of Organization
purpose: Explains organizing and the types of organization, formal and informal.
pace: standard — new vocabulary of organization
style: descriptive
words: 5075
figures: 1
- fig01: an organization chart with lines of authority
plate: a Victorian railway office with a large wall chart of the company's departments
sections:
12.1 | What Organizing Is | new idea | 675
12.2 | The Organizing Process | technique | 900
12.3 | Formal and Informal Organization | framework | 900
concepts:
- 12.1 | organizing | standard | needs: none
- 12.1 | purpose of organizing | standard | needs: organizing
- 12.1 | nature of organizing | standard | needs: purpose of organizing
- 12.2 | identify tasks | standard | needs: nature of organizing
- 12.2 | group tasks | standard | needs: identify tasks
- 12.2 | assign duties | standard | needs: group tasks
- 12.2 | set relationships | standard | needs: assign duties
- 12.3 | formal organization | standard | needs: set relationships
- 12.3 | informal organization | standard | needs: formal organization
- 12.3 | why people join informal groups | standard | needs: informal organization
- 12.3 | making organization effective | standard | needs: why people join informal groups
defines:
- Organizing | Arranging tasks, people and resources into a working structure so that goals can be reached.
- Formal organization | The official structure of jobs, rules and lines of authority that management sets up.
- Informal organization | The network of friendships and social groups that forms among employees on its own.
- Organization chart | A diagram showing the jobs in an organisation and who reports to whom.
- Organizational structure | The pattern of jobs, groups and reporting lines that divides and links the work.
assumes: Management process, Productivity, Effectiveness, Management, Organisation, Manager
words-in-use: chart; structure
case: Duties 2026 (10): buying, receiving, shelving, selling, cashier, accounts, online orders, returns, school accounts, marketing; assigned to 7 people; lunch group of 3 sales assistants exchange book tips informally.
history: Daniel McCallum, 1855: the Erie Railroad's general superintendent drew an early organisation chart | search: McCallum Erie Railroad organization chart 1855
refs: Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — management process; Chapter 1, 'What Management Is', section 1.1 'The Idea of Management' — organisation
need: you-will-need: Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'; Chapter 1 'What Management Is', section 1.1 'The Idea of Management'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: separate formal from informal; think: why informal groups grow; pause: after formal organization
sources: no

=== CH13 ===
title: Structure: Span, Departments and Forces
purpose: Explains how many people a manager can lead, how work is grouped and what forces shape structure.
pace: standard — structural choices
style: descriptive
words: 5075
figures: 1
- fig01: wide and narrow spans shown as two trees
plate: a Victorian customs department with desks in tiers and a chief clerk above
sections:
13.1 | Span of Management | new idea | 675
13.2 | Departmentalization | framework | 1125
13.3 | Forces Behind Structure | framework | 675
concepts:
- 13.1 | span of management | standard | needs: none
- 13.1 | factors for an effective span | standard | needs: span of management
- 13.1 | tall and flat structures | standard | needs: factors for an effective span
- 13.2 | departmentalization | standard | needs: tall and flat structures
- 13.2 | by function | standard | needs: departmentalization
- 13.2 | by product | standard | needs: by function
- 13.2 | by customer | standard | needs: by product
- 13.2 | by territory | standard | needs: by customer
- 13.3 | bureaucratic and behavioural models | standard | needs: by territory
- 13.3 | situational factors | standard | needs: bureaucratic and behavioural models
- 13.3 | division of labour and coordination | standard | needs: situational factors
defines:
- Span of management | The number of employees who report directly to one manager.
- Departmentalization | Grouping jobs into departments according to a common basis, such as function or customer.
- Tall structure | A structure with many levels and small spans of management.
- Flat structure | A structure with few levels and wide spans of management.
- Division of labour | Dividing a large job into smaller tasks done by different people.
assumes: Organizing, Formal organization, Informal organization
words-in-use: department; span
case: Spans 2026: Nurul 2 direct reports; Rafiq 5; grouping by function (sales, stock, accounts) used now; alternative by customer type with 2025 sales: walk-in Tk 9,000,000, schools Tk 3,600,000, online Tk 1,800,000.
history: General Motors, 1920s: Alfred Sloan reorganised GM into divisions for each car brand under central control | search: Alfred Sloan GM divisional structure 1920s
refs: Chapter 12, 'Organizing and Types of Organization', section 12.1 'What Organizing Is' — organizing; Chapter 12, 'Organizing and Types of Organization', section 12.3 'Formal and Informal Organization' — formal structure
need: you-will-need: Chapter 12 'Organizing and Types of Organization', section 12.1 'What Organizing Is'; Chapter 12 'Organizing and Types of Organization', section 12.3 'Formal and Informal Organization'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: choose span for described tasks; think: tall versus flat; pause: after span
sources: no

=== CH14 ===
title: Authority, Delegation and Coordination
purpose: Explains how authority flows, how it is delegated and how coordination holds work together.
pace: slow — authority, power and delegation are easily confused
style: descriptive
words: 5200
figures: 2
- fig01: line, staff and functional authority
- fig02: authority, responsibility and accountability triangle
plate: a Victorian general in a field tent handing a sealed order to a messenger
sections:
14.1 | Authority and Power | new idea | 900
14.2 | Line, Staff and Functional Authority | framework | 675
14.3 | Delegation | technique | 425
14.4 | Centralization and Decentralization | framework | 300
14.5 | Coordination | framework | 300
concepts:
- 14.1 | authority | standard | needs: none
- 14.1 | power | standard | needs: authority
- 14.1 | sources of power | standard | needs: power
- 14.1 | authority versus power | standard | needs: sources of power
- 14.2 | line authority | standard | needs: authority versus power
- 14.2 | staff authority | standard | needs: line authority
- 14.2 | functional authority | standard | needs: staff authority
- 14.3 | delegation | standard | needs: functional authority
- 14.3 | obstacles to delegation | minor | needs: delegation
- 14.3 | responsibility, authority and accountability | minor | needs: obstacles to delegation
- 14.4 | centralization | minor | needs: responsibility, authority and accountability
- 14.4 | decentralization | minor | needs: centralization
- 14.4 | which to choose | minor | needs: decentralization
- 14.5 | coordination | minor | needs: which to choose
- 14.5 | basic coordinating activities | minor | needs: coordination
- 14.5 | structural coordination techniques | minor | needs: basic coordinating activities
defines:
- Authority | The right to give orders and to expect them to be obeyed, given by position.
- Power | The ability to influence other people's behaviour, whether or not one holds a position.
- Delegation | Giving a subordinate the right to make decisions and take action for a stated task.
- Accountability | The duty to explain and answer for results to the person who delegated the work.
- Line authority | The right of a manager to direct the work of subordinates in the chain of command.
- Staff authority | The right to advise and support line managers, without commanding their subordinates.
- Centralization | Keeping most decision-making power at the top of the organisation.
- Coordination | Linking the work of different people and departments so that they pull toward the same goal.
assumes: Organizing, Formal organization, Informal organization, Span of management, Departmentalization, Tall structure
words-in-use: power; authority
case: Delegation from 1 March 2026: Rafiq may approve returns up to Tk 5,000, above that Nurul; Tariq may place purchase orders up to Tk 50,000; weekly meeting each Sunday at 9:00 for 30 minutes. Tariq holds staff authority (advises on cash and credit).
history: Ricardo Semler, 1980s: at Semco in Brazil he gave workers wide decision rights, a famous case of decentralisation | search: Ricardo Semler Semco decentralised
refs: Chapter 12, 'Organizing and Types of Organization', section 12.1 'What Organizing Is' — organizing; Chapter 13, 'Structure: Span, Departments and Forces', section 13.1 'Span of Management' — span
need: you-will-need: Chapter 12 'Organizing and Types of Organization', section 12.1 'What Organizing Is'; Chapter 13 'Structure: Span, Departments and Forces', section 13.1 'Span of Management'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: sort examples of authority; think: delegating returns; pause: after accountability
sources: no

=== CH15 ===
title: Human Factors, Groups and Creativity
purpose: Explains the human factors that shape work and how groups and new ideas arise.
pace: standard — human side of work
style: descriptive
words: 5175
figures: 1
- fig01: individual, group and organisation factors as nested rings
plate: a Victorian workshop with craftsmen at one bench sharing a tool and a lamp
sections:
15.1 | Human Factors in Management | new idea | 1000
15.2 | Groups and Teams | framework | 900
15.3 | Creativity and Innovation | framework | 675
concepts:
- 15.1 | human factors | core | needs: none
- 15.1 | individual differences | standard | needs: human factors
- 15.1 | perception | standard | needs: individual differences
- 15.2 | group | standard | needs: perception
- 15.2 | team | standard | needs: group
- 15.2 | why people form groups | standard | needs: team
- 15.2 | group norms | standard | needs: why people form groups
- 15.3 | creativity | standard | needs: group norms
- 15.3 | innovation | standard | needs: creativity
- 15.3 | encouraging ideas | standard | needs: innovation
defines:
- Human factors | The traits, feelings and relationships of people that affect how they work.
- Group norm | An unwritten rule that members of a group follow and expect others to follow.
- Team | A small group of people with joined skills, working toward a common goal for which they are all accountable.
- Creativity | The ability to produce new and useful ideas.
- Innovation | Turning a new idea into a new product, service or method that is put to use.
assumes: Organizing, Formal organization, Informal organization, Management process, Productivity, Effectiveness
words-in-use: team; norm
case: Suggestions 2026: 14 suggestions in January to June 2026, 3 adopted; reading corner built April 2026 at Tk 45,000; weekend sales rose from Tk 180,000 to Tk 207,000 a month (+15%).
history: 3M, 1968 to 1980: Spencer Silver's weak adhesive and Art Fry's idea led to Post-it notes, launched in 1980 | search: 3M Post-it history Silver Fry
refs: Chapter 12, 'Organizing and Types of Organization', section 12.3 'Formal and Informal Organization' — informal organization; Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — leading
need: you-will-need: Chapter 12 'Organizing and Types of Organization', section 12.3 'Formal and Informal Organization'; Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: tell group from team; think: stopping creative ideas; pause: after norm
sources: no

=== CH16 ===
title: Motivation
purpose: Explains what moves people to work and compares the main theories.
pace: slow — motivation theories are abstract and layered
style: descriptive
words: 5300
figures: 2
- fig01: Maslow's five needs as a stair
- fig02: needs-goal model as a loop
plate: a Victorian pit-head with miners, lamp-room and a pay window
sections:
16.1 | What Motivation Is | new idea | 675
16.2 | Need Theories | defining theory | 1600
16.3 | Theory X and Theory Y | defining theory | 425
concepts:
- 16.1 | motivation | standard | needs: none
- 16.1 | why managers must understand it | standard | needs: motivation
- 16.1 | assumptions about motivation | standard | needs: why managers must understand it
- 16.2 | Maslow's need hierarchy | landmark | needs: assumptions about motivation
- 16.2 | needs-goal model | standard | needs: Maslow's need hierarchy
- 16.2 | two-factor view | standard | needs: needs-goal model
- 16.3 | Theory X | standard | needs: two-factor view
- 16.3 | Theory Y | minor | needs: Theory X
- 16.3 | using the theories | minor | needs: Theory Y
defines:
- Motivation | The inner push that makes a person choose to act and keep acting toward a goal.
- Need | A lack that a person feels and wants to fill.
- Need hierarchy | Maslow's idea that human needs rank in five levels, and a higher need matters only after the lower ones are met.
- Theory X | A set of assumptions that people dislike work and must be directed and watched.
- Theory Y | A set of assumptions that people can enjoy work and seek responsibility.
- Incentive | A reward offered to encourage a person to act in a wanted way.
assumes: Human factors, Group norm, Team, Management process, Productivity, Effectiveness
words-in-use: need; drive
case: Staff record 2025: 2 of 7 employees left; total wages Tk 2,940,000 (average Tk 420,000 a year per employee); 2026 plan: bonus of one month's pay if the sales target is met; cashier offered training for a supervisor post.
history: Abraham Maslow, 1943: A Theory of Human Motivation, in Psychological Review | search: Maslow 1943 A Theory of Human Motivation
refs: Chapter 15, 'Human Factors, Groups and Creativity', section 15.1 'Human Factors in Management' — human factors; Chapter 3, 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process' — leading
need: you-will-need: Chapter 15 'Human Factors, Groups and Creativity', section 15.1 'Human Factors in Management'; Chapter 3 'The Functions of Management and How Management Is Judged', section 3.1 'The Management Process'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: place incentives on Maslow's levels; think: Theory X in a shop; pause: after need hierarchy
sources: no

=== CH17 ===
title: Leadership
purpose: Explains what leadership is and compares styles and sources of power.
pace: standard — styles and power
style: descriptive
words: 5175
figures: 1
- fig01: Tannenbaum and Schmidt leadership continuum
plate: a Victorian ship's captain at the helm in rough sea with crew behind
sections:
17.1 | Leader and Manager | new idea | 1000
17.2 | Approaches to Leadership | defining theory | 675
17.3 | Styles and Modern Ideas | framework | 900
concepts:
- 17.1 | leadership | core | needs: none
- 17.1 | leadership versus management | standard | needs: leadership
- 17.1 | leader power | standard | needs: leadership versus management
- 17.2 | trait approach | standard | needs: leader power
- 17.2 | situational approach | standard | needs: trait approach
- 17.2 | Tannenbaum and Schmidt continuum | standard | needs: situational approach
- 17.3 | autocratic, participative, free-rein | standard | needs: Tannenbaum and Schmidt continuum
- 17.3 | emotional intelligence | standard | needs: autocratic, participative, free-rein
- 17.3 | transformational leadership | standard | needs: emotional intelligence
- 17.3 | coaching leadership | standard | needs: transformational leadership
defines:
- Leadership | The process of influencing people to work willingly toward group goals.
- Leadership style | The usual way a leader makes decisions and treats followers.
- Emotional intelligence | The ability to notice, understand and manage one's own and other people's feelings.
- Transformational leadership | Leading by inspiring followers to look beyond self-interest and to change themselves and the organisation.
- Referent power | Power that comes from other people's liking and admiration for a leader.
assumes: Motivation, Need, Need hierarchy, Authority, Power, Delegation
words-in-use: lead; style
case: Layout change 3 March 2026: Nurul decided and told staff; Rafiq asked the 3 sales assistants first and then decided; both styles used within one week.
history: Ernest Shackleton, 1914 to 1916: led the Endurance expedition, survived loss of the ship and brought all 28 men home | search: Shackleton Endurance 1914 1916 leadership
refs: Chapter 16, 'Motivation', section 16.1 'What Motivation Is' — motivation; Chapter 14, 'Authority, Delegation and Coordination', section 14.1 'Authority and Power' — authority and power
need: you-will-need: Chapter 16 'Motivation', section 16.1 'What Motivation Is'; Chapter 14 'Authority, Delegation and Coordination', section 14.1 'Authority and Power'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: match style to situation; think: leader and manager; pause: after continuum
sources: no

=== CH18 ===
title: Controlling
purpose: Teaches the control process, its types and its needs.
pace: standard — final function that closes the cycle
style: descriptive
words: 5250
figures: 1
- fig01: control process as a loop
plate: a Victorian engine-room with gauges, a governor and an engineer reading them
sections:
18.1 | What Controlling Is | new idea | 675
18.2 | The Control Process | technique | 900
18.3 | Types of Control | framework | 775
18.4 | Making Control Effective | framework | 300
concepts:
- 18.1 | controlling | standard | needs: none
- 18.1 | nature and principles | standard | needs: controlling
- 18.1 | link with planning | standard | needs: nature and principles
- 18.2 | setting standards | standard | needs: link with planning
- 18.2 | measuring performance | standard | needs: setting standards
- 18.2 | comparing | standard | needs: measuring performance
- 18.2 | correcting | standard | needs: comparing
- 18.3 | feedforward control | standard | needs: correcting
- 18.3 | concurrent control | standard | needs: feedforward control
- 18.3 | feedback control | standard | needs: concurrent control
- 18.3 | monitoring | minor | needs: feedback control
- 18.4 | requirements of effective control | minor | needs: monitoring
- 18.4 | barriers to control | minor | needs: requirements of effective control
- 18.4 | power and control | minor | needs: barriers to control
defines:
- Controlling | Checking results against plans and taking action to correct any difference.
- Standard | A level of performance set in advance as the measure of what is acceptable.
- Feedforward control | Control that acts before work begins, to prevent problems.
- Concurrent control | Control that acts while the work is being done.
- Feedback control | Control that acts after the work is done, using the results to correct later work.
assumes: Plan, Planning premise, Alternative, Objective, Management by objectives, Key result area
words-in-use: control; standard
case: Control record: stock loss standard 1% of stock value (Tk 35,000 of Tk 3,500,000); actual 2025 loss Tk 56,000 = 1.6%; correction: security tags at Tk 30,000 fitted in 2026; weekly count of 200 titles.
history: Barings Bank, 26 February 1995: weak control let a trader's losses grow until the bank collapsed | search: Barings Bank 1995 collapse Leeson controls
refs: Chapter 7, 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is' — planning; Chapter 9, 'Objectives and Management by Objectives', section 9.1 'Objectives' — objectives
need: you-will-need: Chapter 7 'Planning: Meaning, Nature and Steps', section 7.1 'What Planning Is'; Chapter 9 'Objectives and Management by Objectives', section 9.1 'Objectives'
ladder: rung 1 a familiar case; rung 2 the same case reshaped; rung 3 a new setting; rung 4 a case that combines earlier ideas
practice: review: place actions as feedforward, concurrent or feedback; think: control that goes too far; pause: after control process
sources: no
