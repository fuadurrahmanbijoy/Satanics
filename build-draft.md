# build-draft

**Role.** Turns one course file into one self-contained chapter-writing guide, `build-chapter.md`. This is the only stage that reads the course file, and the course file never reaches any later stage. Its only deliverable is `build-chapter.md`.

**Start.** Attach this file and the course file (JSON, spreadsheet, or pasted text; missing fields are acceptable). Say: *Start with build-draft.* Optional settings may follow in the same prompt (section 0, step 2).

---

## 0. Run order (agent: follow exactly, add nothing)

1. **Course file present?** If none is attached or pasted, reply "Attach the course file." and stop.
2. **Settings.** Take these from the prompt; use the default when a setting is absent. Never ask about them.

   | Setting | Default |
   | --- | --- |
   | spelling | British |
   | dates | day, month, year (12 March 2026) |
   | country and standards | mixed, no single country: the running case's home is Bangladesh, but examples and history draw on several countries; name the place whenever law or standards differ |
   | currency and numbers | taka for the running case, written *Tk 158,000* (international digit grouping); a historical case keeps its own currency |
   | running case | **Meghna and Sons**, a family-run bookshop, shown as an illustrative documented case file; `case: <other setting>` in the prompt replaces it |
   | size | none (only the per-chapter range in section 4 applies) |
   | tolerance | 10% |
   | words per page | 340 (calibrated on real runs; adjust after the next one) |

   `size` may be given in pages or in words, for example `size: 350 pages`.
3. **Convert the course file to `course.txt` by code** (JSON, spreadsheet or pasted text become plain text). Do not read it into the chat any other way.
4. **Analyse** (section 2). Keep terse working notes in `notes.md`. It is scratch: never print it, never deliver it.
5. **Gate** (section 3). Show the compact plan. Stop. Wait for the reader.
6. **On confirm:** write `g.md` and `e.md` (section 4), assemble (section 5), deliver `build-chapter.md`, then run the checks (section 6) and patch if they fail.
7. **Final message:** one line, for example: *build-chapter.md delivered: 15 chapters.* Add nothing else.

---

## 1. Token rules (apply to every step)

- Read the course file once. Never read a file back after writing it. Never reprint a file.
- Do not restate these instructions, summarise your work, narrate progress, or explain choices.
- No previews, audits, or repeat checks beyond the two scripts in section 6. Scripts print failures only.
- Fix failures with small `str_replace` patches. Never rewrite a whole file to fix a few lines.
- Do arithmetic (word totals, page estimates, case-file figures) by code, not by hand.
- The only text you write in chat is the gate plan, a gate update, and the final line.
- Write the entries file in batches of at most six chapters per tool call (create, then append). This is for reliability, not a recheck.
- Deliver the output as soon as it is assembled. Checks run afterwards. If a check forces a patch, patch the same file in place and deliver it again under the same name (no v2, v3).

---

## 2. Analysis (silent; notes only)

**Standing rules.** The back story (objectives, topics, past questions, marks, years, frequency) never appears in any output. Question frequency and marks never change how large or emphasised a topic is; they affect only examples and end-of-chapter practice.

1. **Inventory.** List the learning outcomes, each with its topics, and the past questions with their sub-questions and, where known, marks, year and topic. Exactly as found; change nothing.
2. **Flatten topics.** Make one list of unique topics. Merge duplicates. Keep the link to the outcome in the notes only.
3. **Profile the questions.** For each: type (define, explain, compare, calculate, apply to a case, discuss), the skill it tests, marks and year if given, and repeated question families. Record frequency and marks; never let them change a topic's size.
4. **Map questions to topics.** Trace each question to one or more topics. Log questions with no topic and topics with no questions. Neither is dropped: an unmapped question joins its nearest topic or creates a concept; a topic with no questions is still taught.
5. **Break topics into concepts.** A concept is one idea the reader must hold. List each concept's prerequisites.
6. **Dependency map.** Order concepts by prerequisite. Decide what an absolute beginner must learn first (vocabulary, everyday bridges) and add it as foundation material even if no topic names it. Foundation chapters are numbered continuously with the rest; there is no separate group.
7. **Form chapters and sections.** Group by teaching logic, so ideas that explain each other sit together: 3 to 6 sections per chapter, 8,000 to 12,000 words per chapter. Order chapters by dependency. Expect roughly 13 to 16 chapters; do not force the number. The first three chapters are the slowest of the book: they teach the reader how to read the book as well as the subject.
8. **Name and clean.** Give chapters and sections plain subject titles. No objective labels, exam years, marks, frequency remarks or copied question wording in any title, summary or note. Settle overlaps between chapters with one cross-reference, never with repetition.
9. **Course type per chapter.** Set `style`: *calculation* (the chapter teaches computed results), *descriptive* (distinctions, institutions, reasoning), or *mixed*. The style decides the example form (section 4 of the method) and whether the book needs the appendix **A1** (see below).
10. **Plan examples and practice.** For each chapter plan the example ladder. Calculation: clean numbers first, then messier ones. Descriptive: a familiar case, a reshaped case, a new setting, then a case that combines earlier ideas. Build the end-of-chapter practice (review questions, "Think it through", and "Pause and check" items after slow passages) from the question families, rewritten in new words with new data. Never copy question wording.
11. **Terms.** Assign every new term to exactly one chapter: the chapter where it is first needed. That chapter defines it; later chapters only assume it. Pick one chosen word for every concept that has several names.
12. **Running case.** Build the whole case once, in chapter order, carrying figures forward (use code for arithmetic). Every chapter entry then receives its own complete, fixed slice, so chapters never depend on one another. The case is a documented case file: dates, figures, decisions, records; no dialogue and no invented feelings; labelled illustrative. The case is Meghna and Sons (or the prompt's `case:`). In a chapter where it cannot be used naturally, set `case: none` rather than forcing it.
13. **Historical case.** For each chapter name one real, dated case (a short factual account of how the idea solved or saved something) and a search hint. Do not state facts you cannot support; the chapter writer verifies the case. **Opener plate:** give each chapter one engraving subject of its own (for example "a clerk at a high desk among ledgers, 1880s"): a concrete Victorian-era scene or still life tied to the chapter's topic, no two alike in the book, no lettering in it.
14. **Appendix A1.** If any chapter is *calculation* or *mixed*, add the entry `A1` "Maths You Will Use" (a one-to-two-page reference, not a lesson: percentages and percentage change, ratios, averages, rearranging an equation, rounding, reading a graph). Descriptive-only courses have no A1.
15. **Silent coverage check.** Confirm every question tied to a chapter can be answered from it. Fix gaps by adding teaching content (a concept or a section), never by quoting the question.

---

## 3. The gate (one stop, before any writing)

Show exactly this, then stop.

```
**Plan: <course>** · <N> chapters · <W> words · ~<P> pages · target <size or "none"> (<within / over by X / under by X>)
Settings: <spelling · dates · country · currency · running case · tolerance>

**1. <Title>** · <style> · <words> w · <pages> p
1.1 <Section> · 1.2 <Section> · 1.3 <Section>
**2. <Title>** · …
…
To fit: <only when outside target ± tolerance: the smallest trims, as section, change, words saved; not applied yet>

Reply `confirm`, or change anything, for example: `size 350 pages` · `ch6 words 7000` · `merge 4+5` · `drop 3.5` · `move ch9 before ch8`.
```

- Mark foundation chapters with "(foundation)" after the title. Include `A1` last when present.
- **Size check.** Chapter words = the sum of its section estimates plus the fixed part (section 4). Pages = total words ÷ words per page + 2 per chapter (opener and blank page) + 50 (front and back matter, glossary, answers, index). Compute by code. Flag any chapter outside 8,000 to 12,000 words. Compare the book with `size ± tolerance`.
- **After a change:** apply it, then show only the header line and the changed chapters. Ask nothing else. Repeat until the reader says `confirm`.
- Do not proceed to writing without `confirm`.

---

## 4. What to write

Write two files, then assemble.

**`g.md`**

```
## GLOBAL
course: <name as in the course file>
book: <working title if the course file gives one, else the course name>
spelling: … · dates: … · country: … · currency and numbers: …
running case: <label> — illustrative. Base facts (FIXED): <dates, figures, decisions that start the case>
chosen words (one term, one word):
- use <word> — also called <other words>
size plan: <N> chapters, ~<W> words, ~<P> pages
chapters:
1. <Title>
2. <Title>
… (A1. Maths You Will Use, when present)
```

**`e.md`**: one entry per chapter, in order, each starting with its marker line. Entries are compact and self-contained; a chapter writer reads only its own.

```
=== CH07 ===
title: <plain subject title>
purpose: <one line>
pace: slow | standard — <reason>
style: calculation | descriptive | mixed
words: <total>
figures: <N>
- fig01: <what it shows>      (one line per figure; N = 0 means none, and is a decision, never an accident)
plate: <engraving subject for the opening plate; unique in the book; A1 has none>
sections:
7.1 | <title> | <kind: new idea / framework / technique / defining theory> | <section words>
concepts:
- 7.1 | <concept> | <tier: minor / standard / core / landmark> | needs: <earlier concepts or none>
defines:
- <Term> | <plain definition, about 20 words, simpler than the term>
assumes: <terms from earlier chapters, names only, or none>
words-in-use: <at most three everyday words with a special meaning, or none>
case: <FIXED facts and figures for this chapter's step in the running case, complete and self-contained, including every carried-forward figure it needs; "none" if unused>
history: <the real, dated case and a search hint>
refs: <Chapter N, 'Title', section N.N 'Title' — why it connects; or none>
need: <you-will-need: earlier ideas by chapter and section name, or "nothing">
ladder: <the four rungs for this chapter's style>
practice: review: <question families, new words and data>; think: <one problem or case>; pause: <after which slow passages>
sources: yes | no     (yes only for a defining topic with a known origin)
```

- Chapter codes are two digits: `CH01` to `CH99`. The appendix is `=== A1 ===`, with the same keys except `plate:`.
- Sections per chapter: 3 to 6. The opening puzzle is not a section.
- **Words rule.** Section words = the sum of its concepts' tier midpoints (minor 100, standard 225, core 550, landmark 1,150). A concept that is new and abstract moves up one tier. Chapter words = the sum of section words + 2,600 (opening puzzle 400, worked case or examples 700, closing pack and practice 1,500). `A1` is about 1,000 words and has no fixed part.
- Cross-references use chapter number, chapter title, section number and section title only. Never page numbers.
- No course-file wording anywhere. Titles are plain subject titles.

---

## 5. Assembly (by code; never retype the template blocks)

Run from the folder that holds this file, `g.md` and `e.md`. Use this file's actual name in place of `build-draft.md` if it differs.

```
python3 - <<'PY'
import re
src = open('build-draft.md', encoding='utf-8').read()
def blk(n):
    return re.search(r'^<<<%s>>>\n(.*?)^<<<END>>>\n' % n, src, re.S | re.M).group(1)
g = open('g.md', encoding='utf-8').read()
e = open('e.md', encoding='utf-8').read()
out = (blk('CHAPTER-START') + '\n' + g.rstrip() + '\n\n' + blk('CHAPTER-METHOD')
       + '\n<<<CHECK>>>\n' + blk('CHAPTER-CHECK') + '<<<END>>>\n\n' + e.lstrip())
open('build-chapter.md', 'w', encoding='utf-8').write(out)
PY
```

Then deliver `build-chapter.md` (in a chat environment, save it to the outputs folder and present it).

---

## 6. Checks (after delivery; code only; failures only)

Save the script below as `catcheck.py` and run `python3 catcheck.py build-chapter.md course.txt`. It prints `ok` or a list of failures. Patch each failure in place, deliver the same file again, and do not rerun unless a patch touched more than a few lines. It checks: course wording in the output, copied question wording (eight-word runs), required keys, section counts, unique term ownership, valid cross-references, and chapter sequence.

```
import re, sys
cat, course = sys.argv[1], sys.argv[2]
C = open(cat, encoding='utf-8').read()
S = open(course, encoding='utf-8', errors='ignore').read()
bad = []
EXAM = re.compile(r"\b(past papers?|past questions?|learning (?:objectives?|outcomes?)|course (?:objectives?|outcomes?)|syllabus|exam(?:ination)? (?:papers?|years?|questions?)|marking scheme|marks? (?:allocated|awarded|weighting)|course file)\b", re.I)
a, b = C.find('## GLOBAL'), C.find('## METHOD')
e = re.search(r'^=== ', C, re.M)
if a < 0 or b < a or not e: print('structure broken: GLOBAL, METHOD or entries missing'); sys.exit(1)
scan = C[a:b] + C[e.start():]
for n, ln in enumerate(scan.splitlines(), 1):
    for r in EXAM.finditer(ln): bad.append('course wording "%s": %s' % (r[0], ln[:60]))
tok = lambda t: re.findall(r"[a-z0-9']+", t.lower())
cw = tok(S)
sh = {' '.join(cw[i:i + 8]) for i in range(len(cw) - 7)}
sw = tok(scan)
i, hits = 0, []
while i < len(sw) - 7:
    g = ' '.join(sw[i:i + 8])
    if g in sh: hits.append(g); i += 8
    else: i += 1
for g in hits[:5]: bad.append('copied wording: ' + g)
KEYS = ('title', 'purpose', 'pace', 'style', 'words', 'figures', 'sections', 'concepts', 'defines', 'assumes', 'case', 'history', 'refs', 'need', 'ladder', 'practice', 'plate', 'sources')
codes, seen, texts, plates = [], {}, {}, {}
for en in re.split(r'^=== ', C[e.start():], flags=re.M)[1:]:
    head, _, body = en.partition('\n')
    code = head.split()[0]
    codes.append(code); texts[code] = body
    pl = re.search(r'^plate:[ \t]*(.+)$', body, re.M)
    if pl:
        if pl[1].strip().lower() in plates: bad.append('%s: plate subject repeats %s' % (code, plates[pl[1].strip().lower()]))
        plates[pl[1].strip().lower()] = code
    for k in KEYS:
        if k == 'plate' and code == 'A1': continue
        if not re.search(r'^%s:' % k, body, re.M): bad.append('%s: missing %s:' % (code, k))
    n = len(re.findall(r'^\d+\.\d+ \|', body, re.M))
    if code.startswith('CH') and not 3 <= n <= 6: bad.append('%s: %d sections (need 3-6)' % (code, n))
    dm = re.search(r'^defines:[ \t]*\n((?:- .*(?:\n|\Z))+)', body, re.M)
    for x in (dm.group(1).splitlines() if dm else []):
        t = x[2:].split('|')[0].strip().lower()
        if t in seen: bad.append('%s: term "%s" already defined in %s' % (code, t, seen[t]))
        seen[t] = code
chs = [c for c in codes if c.startswith('CH')]
if chs != ['CH%02d' % i for i in range(1, len(chs) + 1)]: bad.append('chapter codes are not CH01..CHnn in order')
for code, body in texts.items():
    for n in set(re.findall(r'Chapter (\d+)', body)):
        if 'CH%02d' % int(n) not in codes: bad.append('%s: reference to missing Chapter %s' % (code, n))
print('\n'.join(bad) if bad else 'ok')
```

---

## 7. Template blocks (copied by code into `build-chapter.md`; do not edit or retype)

<<<CHAPTER-START>>>
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
<<<END>>>

<<<CHAPTER-METHOD>>>
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
<<<END>>>

<<<CHAPTER-CHECK>>>
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
