---
name: visual-study-notes
description: Turn study material into richly structured, visually engaging notes — proper headings, subtopics, divider lines, diagrams where they genuinely help, and matching emojis. Works from pasted lessons, chapters, articles, video/lecture transcripts, or just a bare topic name. Use whenever the user pastes lesson/chapter/transcript/article content and wants "notes", "study notes", "notes with diagrams", or a "study guide" — and also when they only give a topic name, even without saying "notes" (e.g. "make notes on TCP/IP", "explain photosynthesis with a diagram"). Also produces a companion interactive HTML mind map of the same content for embedding in Notion. Always asks upfront whether to write in English or Marathi. Notes render directly in the chat reply; the mind map is saved as a downloadable HTML file.
---
 
# Visual Study Notes
 
Turn source material — or just a topic name — into notes that actually look like something worth studying from: clear structure, a diagram exactly where one clarifies the idea, and enough personality (emojis, dividers) that flipping through them doesn't feel like reading a wall of text.
 
## Step 1: Ask which language, before anything else
 
Before touching the source material, ask which language the notes should be written in — English or Marathi, exactly two options, nothing else to choose. Use `ask_user_input_v0` (or a plain one-line question if that tool isn't available) and wait for the answer before moving on. Don't guess this from the input language or skip the question even if the source text or topic is obviously in one language or the other — always ask.
 
## Step 2: Figure out what you're working with
 
**Pasted content** (chapter, article, transcript, lecture notes, long text): this is the primary source of truth. Build the notes from it. Don't invent facts that aren't in there — if something is unclear or incomplete in the source, note it briefly rather than filling the gap with a guess.
 
**Bare topic name, no pasted text** (e.g. "make notes on the Cuban Missile Crisis", "notes on RAG in AI"): before writing anything, run a few web searches to ground the notes in current, accurate information — don't rely purely on memory, even for topics that feel well-known. This matters most for anything with numbers, dates, current status, or fast-moving fields (technology, current affairs), where memory alone can be stale or subtly wrong.
 
## Step 3: Write in the chosen language
 
Write the whole set of notes in whichever language was picked in Step 1, regardless of what language the source material or topic came in. Keep specific/technical terms (proper nouns, code, established English terminology) untouched rather than force-translating them, the same way you would in a translation task — a translated term that no textbook uses is often less useful than the original.
 
## Step 4: Build the structure
 
Use this shape as a default, adapting depth to how much material there actually is (a short topic doesn't need five sections just to hit a template):
 
```markdown
# 🎯 [Topic Title]
 
> One or two lines: what these notes cover and why it matters.
 
---
 
## 📌 [First major section]
 
Explanation in full sentences, not just fragments — detailed enough that someone could
learn the concept from this alone. **Bold** the terms that matter most.
 
When a concept is abstract, ground it with a short real-world example or analogy (1-2
sentences) the way a good teacher would — enough to make it stick, not so much that it
turns into a story. Skip this where the concept is already concrete.
 
### [Subtopic, if the section has real internal structure]
- Point
- Point
 
*(diagram here if this section's content is better seen than read — see Step 5)*
 
---
 
## 🔑 [Next major section]
...
 
---
 
## 📚 Source
[See Step 6]
```
 
**On density:** default to detailed, explanatory notes rather than terse exam-cram bullets — full sentences that teach the idea, with brief grounding examples where useful. Bullets are for genuine lists (steps, components, criteria), not a substitute for explaining a concept.
 
**On dividers:** a `---` line between major (`##`) sections only — not between every subtopic, or the notes start to feel chopped up instead of organized.
 
**On emojis:** one emoji per heading that actually reflects its content (🌱 for growth/biology, ⚙️ for mechanisms, 🔑 for key concepts, 📊 for data, ⚔️ for conflict/history, etc.) — not decoration for its own sake, and not sprinkled mid-sentence. If a good matching emoji doesn't come to mind, it's fine to skip it rather than force one.
 
## Step 5: Add a diagram only where it earns its place
 
Decide per-section, not per-notes-document — most sections need zero diagrams, one or two well-chosen ones beat a diagram under every heading. A diagram earns its place when the content is genuinely about *structure, sequence, or relationship* — something spatial that a paragraph would have to work hard to convey.
 
Use fenced ` ```mermaid ` code blocks — they render as real inline diagrams directly in the chat, no separate file needed, and the same code block works if these notes get pasted into Notion (Notion has a native Mermaid code block too). Stick to `graph TD` / `graph LR` flowcharts for everything, rather than mermaid's newer diagram types:
 
| Content is about... | Diagram type |
|---|---|
| A process or sequence of steps | `graph TD` or `graph LR` flowchart |
| Cause → effect chains | flowchart with labeled arrows |
| Categories and sub-categories | `graph TD` flowchart, root node branching down into category nodes |
| Events over time | `graph LR` flowchart, nodes chained left to right in chronological order |
| Comparing 2+ things across criteria | a markdown table (not a diagram) |
 
Deliberately avoid `mindmap` and `timeline` syntax even though they exist in mermaid — Notion's bundled mermaid version renders them inconsistently (sometimes as plain text instead of a diagram), while plain flowcharts render reliably everywhere: Notion, GitHub, and this chat. A flowchart tree conveys the same hierarchy just fine.
 
If nothing in a section is spatial/relational, don't force a diagram in — a well-written paragraph is the right call there.
 
## Step 6: Always close with a Source line
 
The very last thing in the notes, after a final `---`:
 
```markdown
## 📚 Source
[what it's based on]
```
 
- Pasted content → name what it was: `Source: user-provided chapter on [topic]` / `Source: video transcript on [topic]`.
- Bare topic name → `Source: compiled from web research` plus the 2-3 sources actually used (name/domain is enough, full citation isn't needed since this renders in chat, not a document).
- Mixed (pasted content + a search to verify or fill a gap) → mention both.
## Step 7: Also build a companion HTML mind map
 
The chat notes above aren't the only output — this skill also produces a second, separate deliverable: an interactive HTML mind map of the same material, built for embedding into Notion (Notion's HTML/embed option can host it).
 
To make it, read and follow `/mnt/skills/user/dynamic-html-mind-map-architect/SKILL.md` exactly as written — its archetype selection, node/edge extraction, and HTML template are all already tuned and shouldn't be second-guessed or modified here. Feed it the same source material you used for the notes (the pasted content, or what you gathered from web research for a bare topic). Its title should match the notes' title from Step 4.
 
That skill outputs a single self-contained HTML file — save it with `create_file` to `/mnt/user-data/outputs/` and share it with `present_files` so the person can download it and drop it into Notion. This happens every time this skill runs, right after the chat notes, not as an optional extra.
 
## A note on tone
 
These are meant to read like notes a sharp, slightly playful classmate wrote — clear and correct first, characterful second. If a choice ever pits accuracy against visual flair (an emoji that doesn't quite fit, a diagram forced into a section that didn't need one), accuracy and clarity win every time.
