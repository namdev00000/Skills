---
name: marathi-text-summarizer
description: Summarize text-based input (video transcripts, blog posts, speeches, notes, or any pasted text) into a detailed but simple-vocabulary Marathi summary, optionally paired with a mermaid mind map. Use this skill whenever the user pastes a transcript, article, speech, or long text and asks for a summary, a Marathi summary, or a mind map of the content — even if they don't use the word "summarize" explicitly (e.g. "explain this video's content", "give me the gist of this in Marathi", "mind map बनवा"). Always trigger this when the user asks for output specifically in Marathi, regardless of what language the source text is in.
---
 
# Marathi text summarizer
 
Turns any text input into a detailed, simple-language Marathi summary, plus an optional mermaid mind map.
 
## Step 1: Normalize the input
 
Treat every input type the same way — reduce it to plain text before summarizing:
- Video transcript → treat as spoken text, ignore timestamps/speaker tags
- Blog / article → strip formatting, keep the argument and key facts
- Speech → treat as spoken text
- Notes / thoughts → treat as-is, tighten loose phrasing
If the input isn't in-context text (e.g. it's a file), read it first before proceeding.
 
## Step 2: Write the Marathi summary
 
Rules for the summary:
- **Always in Marathi**, regardless of the source language.
- **Detailed** — cover all the main points and their supporting details, not just a one-line gist. Don't compress so hard that nuance is lost.
- **Simple vocabulary** — use short, everyday Marathi words. Avoid literary/Sanskrit-heavy (शुद्ध/पंडिती) words when a common word works. Write like you're explaining it to a friend, not writing a textbook.
- **Use analogies and everyday examples** wherever they help the concept land — especially for abstract, technical, or numeric ideas. Pull the analogy from daily life (money, food, family, travel, sports, weather) rather than another technical domain. Keep it to one well-chosen analogy per concept, not a string of them — a forced or overloaded analogy is worse than none. Skip this for content that's already concrete and simple (e.g. a short factual note) where an analogy would just add length without adding clarity.
- **Technical or English-origin terms** (e.g. mitochondria, inflation, API, blockchain) stay in English, written inline in the Marathi sentence — don't force a translation and don't track a running glossary across the conversation. Example: "mitochondria पेशीचे ऊर्जा केंद्र आहे." (If the user ever asks for the bracket style instead — "पेशीचे ऊर्जा केंद्र (mitochondria) आहे" — switch to that for the rest of the conversation.)
- Structure with short paragraphs or bullet points if the source has multiple distinct sections — whichever reads more naturally for that content.
## Step 3: Decide whether a mind map is needed
 
Build a mermaid mind map when the content has enough internal structure to actually branch — i.e. it breaks into distinct topics, and at least some of those topics have their own sub-points worth showing.
 
Skip the mind map when:
- The text is short or already simple (a single idea, a short note, a short paragraph)
- The content is one continuous line of reasoning that doesn't split into separate topics at all
- The user explicitly says to skip it
If genuinely unsure, produce the summary and ask the user in one line whether they'd like a mind map too, rather than guessing.
 
### Mind map shape follows the content — don't force a template
 
Don't default to a fixed shape (e.g. always 3-4 branches, always 2 levels deep). Instead, let the structure of the source content dictate the map's shape:
 
- **Few main ideas, little internal detail** ("starfish" shape) — a handful of branches straight off the root, each with 0-2 short sub-points, no deeper nesting. Use this when the content is a flat list of points with not much substructure to each one.
- **Rich, layered content** (deeper "tree" shape) — when a branch itself has categories within it, nest further: root → branch → sub-branch → node, going as many levels deep as the content actually supports. Don't flatten a genuinely hierarchical topic (e.g. "causes" that themselves split into "short-term causes" and "long-term causes") into one flat level just to keep the map simple.
- **Uneven branches are fine** — one branch can go 3 levels deep while another next to it is a single leaf node, if that's what the source material actually looks like. Don't pad thin branches or artificially cap rich ones to make the map look symmetric.
The only real constraint: every node should represent something genuinely present in the content — never add a branch, sub-branch, or node just to fill out a shape. If you're unsure whether something deserves its own sub-branch or should just be a leaf node, ask: does this point itself have parts, or is it a single, atomic idea? Parts → sub-branch. Atomic → leaf node.
 
### Mind map format
 
Use mermaid `mindmap` syntax, root node in Marathi, all branches/sub-branches/nodes in Marathi (keep technical terms in English inline, same as the summary). Keep each node label short (a few words) — put any extra explanation in the summary text, not in the mind map itself.
 
Flat / starfish example (few branches, shallow):
```
mindmap
  root((मुख्य विषय))
    मुद्दा १
    मुद्दा २
      उप-मुद्दा
    मुद्दा ३
```
 
Deep / layered example (branch has its own sub-branches):
```
mindmap
  root((मुख्य विषय))
    कारणे
      अल्पकालीन
        मुद्दा
        मुद्दा
      दीर्घकालीन
        मुद्दा
    परिणाम
      मुद्दा
```
 
Render it as a mermaid diagram (via the visualization/widget tool if available, or as a fenced ```mermaid code block otherwise) — don't hand-draw it as a manual SVG flowchart.
 
## Notes
 
- No running glossary of technical terms is maintained across the conversation — translate consistently within a single response, but don't build or reference a persistent term list.
- If input is ambiguous about which output the user wants (summary only vs. summary + mind map), default to summary only, and offer the mind map as a follow-up.
 
