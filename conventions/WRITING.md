# Writing

This governs the prose we write: docs, READMEs, commit messages, issues, pull requests, and the wording of any comment that COMMENT.md keeps. One rule sits above the rest: write the simplest true version a junior engineer can read, in as few words as it takes, with an example where one helps.

## Write for a junior engineer

Aim every explanation at a capable engineer who is new to the domain. Assume intelligence, not context. Do not lean on knowledge they would not have yet. Define a term the first time it appears, or avoid it. Writing for the newcomer also serves the expert, who just reads faster; writing for the expert loses the newcomer entirely.

## Simplicity is the proof of understanding

Knowing a domain well is being able to describe it simply, at the reader's level, without losing what matters. Tangled writing is usually incomplete understanding in the costume of depth. If you cannot make it simple, you do not yet understand it well enough. Simplify the words, never the substance: keep every detail the reader needs, drop every word they do not.

## Say it in as few words as possible

Length is a cost the reader pays. Cut filler, hedging, and throat-clearing. Prefer the short word, the short sentence, one idea per sentence. The target is not "brief", it is "nothing left to remove".

- "in order to" becomes "to"
- "it should be noted that the cache is shared" becomes "the cache is shared"
- "we utilize" becomes "we use"

## Show an example

Most people learn a rule faster from one example than from three sentences about it. Pair a concept, a rule, or an API with a small, concrete example whenever it earns its space. An example is often the shortest correct explanation.

Do not then restate in prose what the example plainly shows. Treat an example like a picture, taken in at a glance; point at a detail only when the example is complex enough that the reader might miss it. Restating the obvious is noise, like writing that the sky is blue.

## Tone is professional and plain

Direct, neutral, respectful. No hype or marketing ("blazingly fast", "the best"), no jokes that will age badly, no condescension ("obviously", "just do X"). State things, do not sell them.

## Mechanics

- No emojis, anywhere. They render in color and we cannot control how. The monochrome marks a CLI prints (a check, an arrow, a dot) are a terminal interface, so they live in code, never in prose.
- No "label: explanation" colons in prose; write a sentence. A colon may still introduce a list or a code block. A real key-value (a CLI line like `pass: ok`, a config entry) is an interface, so it belongs in code.
- No em-dashes; use a comma, a semicolon, or two sentences.
- Active voice, present tense ("the parser rejects X", not "X will be rejected by the parser").
- No exclamation marks.
- Cut weasel words like "very", "really", "quite", "a number of".
- Quoted text is exempt. Reproduce an error string, command output, or citation exactly, even if it contains an exclamation mark or an em-dash. Editing a quote makes it false; fix the source instead.

## Example

Before:

> In order to get started, you'll simply want to leverage our blazingly-fast client, which, it should be noted, abstracts away basically all of the complexity for you so you can hit the ground running in no time.

After:

> The client handles auth, retries, and pagination. Create one and call it:
>
> ```ts
> const users = await createClient({ apiKey }).users().list();
> ```

The rewrite is shorter, makes no claim it cannot back, reads at a junior level, and shows the thing instead of praising it.
