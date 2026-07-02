---
name: marathi-english-translation
description: Personal Marathi-English translation aid for a learner who understands Marathi more deeply than English. Auto-detects direction (Devanagari script → English, Latin script → Marathi), always gives a full literal translation, never summarizes or shortens on its own, keeps unknown/specific/technical words untouched instead of guessing, and maintains a growing personal glossary of words the user has struggled with. Use this skill whenever the user pastes Marathi or English text and asks to translate it, whenever they mention "translate," "marathi," or share a glossary/dictionary file, and for any input size from a single word up through full essays or 100+ line documents.
---

# Marathi-English Translation (Personal Learning Aid)

This is not a generic machine-translation tool. It exists to help the user, who is
significantly more fluent in Marathi than in English, deepen understanding of English
material by translating it into Marathi terms they already grasp — and vice versa when
they hand you Marathi text. Accuracy of understanding matters more than literary polish.

## Core rules (apply to every request, regardless of length)

1. **Auto-detect direction — never ask.**
   - Input contains Devanagari script (Unicode range U+0900–U+097F) → translate to English.
   - Input is Latin script → translate to Marathi.
   - If a single request mixes both scripts, translate each language-typed chunk into the
     other language, chunk by chunk, keeping the original order.

2. **Always translate in full. Never summarize or shorten.**
   - This applies at every scale: one word, one sentence, 100 lines, a full essay.
   - Do not compress, paraphrase into "the gist," or drop sentences — even for very long
     input — unless the user explicitly asks for a summary in that specific request.
   - If the user does ask for a summary, treat that as a separate, explicit request layered
     on top of translation — not the default behavior.

3. **Never guess at specific/technical/unknown words. Preserve them as-is.**
   - If a word is a proper noun, a domain-specific term, an acronym, or something you're not
     confident has a clean equivalent, leave it untranslated in its original script, inline,
     rather than inventing a shaky translation.
   - Do not silently skip it either — it should visibly remain in the output exactly where
     it occurred.

4. **Check the personal glossary before translating.**
   - See "Personal Glossary" below. If a word/phrase already has an entry, use the user's
     saved meaning instead of a fresh generic translation — consistency for the user's own
     understanding matters more than textbook correctness.

5. **Log new unknown words automatically.**
   - Any word handled under rule 3 (kept as-is) gets added to the glossary automatically —
     no need for the user to ask. See below for format.

## Personal Glossary

The user is building a personal Marathi↔English glossary over time. This is **not** a
standard dictionary — entries capture the user's own understanding, in their own words,
which may differ from the textbook meaning. Once a word has a saved entry, always prefer
it over a fresh lookup for consistency.

**Where it lives:** Since this skill can't write to its own installed folder, the glossary
is a plain markdown file the user keeps and re-uploads at the start of a session (e.g.
`marathi_glossary.md`). If they don't have one yet, start a fresh one.

**Workflow:**
- At the start of a session, check if the user has uploaded a glossary file. If yes, read
  it before translating and use it to resolve any matching words.
- If no glossary file is present, proceed with translation as normal, but still track new
  unknown words in memory for that session.
- After completing the translation, if any new words were added (or if this is the first
  time), output the **updated glossary file** (via create_file, in the same format) so the
  user can download it and re-upload it next time.
- Only add a word once. If it reappears, don't duplicate the entry.

**Entry format** (one per line, under a `## Words` heading):
```
- मराठी_शब्द / english_word — user's own understanding or context noted, if known;
  otherwise mark as "meaning not yet confirmed"
```

If the user gives you their own explanation of a word during the conversation (e.g. "this
one means X to me"), save that explanation verbatim rather than a generic definition.

## Output format by input size

- **Single word / short phrase:** Give the translation directly, plus a one-line note if
  it was resolved via the glossary or newly logged as unknown.
- **Sentence(s):** Translate in full. No added commentary unless something was kept as-is
  (flag those inline, e.g. with the original word in brackets).
- **Multi-line text / essays / 100+ lines:** Translate the entire block, preserving line
  breaks and paragraph structure so it's easy to compare against the original. Do this as
  a complete pass — don't stop partway or offer to continue unless the output is so long
  it must be split for length limits, in which case say so explicitly.
- At the end of any response where new glossary entries were added, briefly note how many
  new words were logged and share the updated glossary file.

## What this skill deliberately does NOT do

- Does not ask the user which direction to translate — direction is always inferred from
  script.
- Does not shorten, summarize, or "clean up" long text by default.
- Does not silently drop words it's unsure about — always keep + log instead.
- Does not overwrite the user's own saved meaning for a word with a generic one.
