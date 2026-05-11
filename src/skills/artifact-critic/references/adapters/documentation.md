# Adapter: documentation

Validates written documentation artifacts — including markdown docs, architecture docs, diagrams with text, READMEs, and any other prose-based artifact — with a focus on language correctness, clarity, and structural completeness.

## Focus questions

- Is the language grammatically correct and free of spelling errors?
- Is the writing clear and unambiguous for the intended audience?
- Is terminology consistent throughout the document?
- Are technical terms used correctly and precisely?
- Is the document structure logical and easy to navigate?
- Are instructions or steps complete and actionable?
- Does the tone match the document's purpose (technical, user-facing, internal)?
- Are acronyms and abbreviations defined on first use?

## Common failure patterns

- Passive voice overuse making ownership and responsibility unclear.
- Inconsistent terminology for the same concept (e.g., "endpoint" vs "route" vs "API path").
- Missing context — document assumes knowledge the reader may not have.
- Instructions missing edge cases or preconditions.
- Headings that do not reflect the actual content beneath them.
- Broken or incomplete sentences caused by copy-paste or partial editing.
- Mixed language register (formal and informal in the same document).
- Untranslated fragments or code-switched phrases when a single language is expected.

## Language quality checks

- **Spelling**: all words correctly spelled; no typos.
- **Grammar**: sentences are complete, subject-verb agreement holds, tense is consistent.
- **Punctuation**: commas, periods, and colons are used correctly.
- **Clarity**: each sentence expresses a single idea; no run-on sentences.
- **Conciseness**: no redundant phrases that add length without meaning.
- **Consistency**: same term used for the same concept throughout.
- **Tone**: matches intended audience (e.g., technical peers, end users, stakeholders).
- **Completeness**: all sections contain meaningful content; no placeholder text remains.

## Extra acceptance conditions

- No spelling or grammatical errors remain.
- Technical terminology is consistent and precisely used.
- Acronyms are defined on first use.
- Document structure matches its stated purpose.
- No placeholder text or TODO markers remain in the output.
- Reading level is appropriate for the target audience.
