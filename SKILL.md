---
name: sentence-surgeon
description: Rewrite, reorganize, tighten, or diagnose prose so every sentence serves the Reader, using June Casagrande's sentence-craft method. Use when asked to rewrite, edit, revise, tighten, polish, clean up, copyedit, proofread, shorten, simplify, de-jargon, punch up, fix the grammar of, or make clearer any prose for human readers — memos, emails, articles, essays, fiction, marketing copy, press releases, blog posts, query letters — or to explain why a piece of writing reads poorly or "sounds off." Not for code, code comments, API/reference docs, UI strings, or brand-governed copy with its own style guide.
---

# Sentence Surgeon

A deterministic, multi-pass editing method distilled from June Casagrande's *It Was the Best of Sentences, It Was the Worst of Sentences* (Ten Speed Press, 2010). It rewrites prose sentence by sentence around one governing doctrine:

> **The Reader is king. You are his servant.** A sentence works only if the Reader — not the writer — gets value from it. Every rule below is a tool for serving the Reader, never a law. "If you're going to use long sentences, it should be by choice, not due to bumbling ineptitude."

## What this skill does

Given any prose text, run an ordered six-pass pipeline: diagnose each sentence's point, rebuild sentence architecture so the main clause carries the main news, fix modifier placement and reference, cut fat and vague words, correct mechanics, then read the whole as the Reader. The full procedure, with a complete worked example, lives in `references/pipeline.md`; the condensed detect cues for all passes live in `references/quick-scan.md` — **"How to work" routes LIGHT EDIT through the quick scan and FULL REWRITE / DIAGNOSE through the full pipeline.**

## Operating modes — pick one before starting

| Mode | When | Contract |
|---|---|---|
| **DIAGNOSE** | User asks "what's wrong with this?" or wants a critique | Run all passes read-only. Report the sentence ledger first, then findings ordered by severity: sentence number, rule violated, quoted evidence, severity, one-line fix direction. Severity scale — **blocker**: meaning or credibility errors (iconic errors, danglers, ambiguous pronouns, agreement, factual incoherence); **major**: structure and lead violations (buried news, upside-down subordination, passive pileups, cramming); **minor**: fat and style (redundant modifiers, flabby phrases, vague words). Change no text. |
| **LIGHT EDIT** | User wants their text cleaned up but it must stay *their* text | Fix violations in place. Preserve sentence order, imagery, register. May split sentences, never merge or reorder facts across sentences. Ties go to the author. |
| **FULL REWRITE** | User wants the best possible version | Reorganize freely for the Reader: rebuild leads, reorder facts, delete writer-serving content (logged). Ties go to the Reader. |

Default to LIGHT EDIT when the user's intent is unclear and the text is theirs; default to FULL REWRITE for anonymous/institutional copy (press releases, memos, marketing). If the request is ambiguous and consequential, ask.

**Invariants, all modes:**
- Never invent facts — only redistribute information already in the source; where specifics are missing, emit `[AUTHOR CHECK: ...]` instead of guessing. Beware quiet inventions: narrowing a vague term ("resources" → "staff"), adding a start date, or promoting a presupposition to an assertion all count.
- Preserve epistemic strength: a hedged claim in the source ("is expected to") stays hedged in the rewrite ("should") — cutting the flabby frame must not upgrade a prediction into a promise, or reported speech into asserted fact.
- Text inside quotation marks is verbatim — trim to a partial quote, paraphrase outside the quotes, or leave it; never edit in-quote (fiction dialogue excepted; see `references/concision.md`).
- Never silently change charged word choices or claims; when cutting a named source or attribution, say where their content went and flag `[AUTHOR CHECK]`.
- Report what you changed and why. State numeric claims (word counts) only if computed exactly; otherwise say "cut by about a third."
- Headings, salutations, signatures, and bulleted fragments are display units, not sentences — don't "complete" them (see `references/pipeline.md` Pass 1 scoping).

## The six passes (summary — full detail in references/pipeline.md)

Order matters: structure first (fine-grained fixes are wasted on sentences about to be rebuilt), placement before word cuts (you can't judge a modifier until it's attached to the right word), mechanics on final wording, Reader read-through on the assembled whole.

1. **X-Ray & Intent Ledger** — for every sentence: isolate main subject + main verb; state the point in one line; ask *Whom is this about? What did they do? Does the Reader care?* Flag sentences whose main clause fails to carry their point. No text changes. → `references/pipeline.md` Pass 1
2. **Structural Rebuild** — main news into the main clause; unstack subordinate chains; unbury nominalizations and "the + gerund + of"; convert or deliberately keep passives; uncram overloaded sentences; fix parallels; simplest workable tense. → `references/structure.md`, `references/reader-first.md`
3. **Placement & Reference** — modifiers next to what they modify; un-dangle participles; at most one relative clause per sentence; every pronoun points at exactly one antecedent. → `references/modifiers.md`
4. **Fat & Word Choice** — deletion-test every modifier; cut flabby phrases, hedges, redundancies; replace vague words with specific ones; audit literal meaning; "said" for attribution. → `references/concision.md`, `references/reader-first.md`
5. **Mechanics & Consistency** — punctuation, tense consistency, run-ons, pronoun case, the iconic credibility-killing errors (its/it's, could of, affect/effect...). → `references/grammar-mechanics.md`
6. **Reader Read-Through** — reread the whole as the Reader: does it lead with Reader impact, vary rhythm, preserve every fact of the original, and still sound like the author? Run the verification checklist in `references/pipeline.md` §7 before returning.

## Voice guard (anti-dogma mechanism)

Before Pass 1, record the text's fingerprint: register, person/tense scheme, genre, signature devices (deliberate fragments, repetition, irony, parentheticals-as-voice). A device that recurs is presumed a choice. Before applying any rule, ask: is this violation plausibly deliberate craft? If yes, draft both versions and keep the one that serves the Reader better — and remember keeping is a decision: a recurring fingerprint device needs only a changelog note, but a single-instance judgment call kept as "deliberate style" gets an `[AUTHOR CHECK]` asking the author to confirm intent, and structural keeps (upside-down subordination, stacked relatives, buried leads) get the check in LIGHT EDIT even when they recur.

Craft writing (content-first: business, news, nonfiction, genre fiction) applies the rules firmly; art writing (form-first literary prose) gets wide exemptions — "It's a mess, but it's supposed to be a mess." Classify by signals, not vibes: publication context and purpose (a memo informs; a literary magazine performs), the density of deliberate devices in the fingerprint, and the user's stated intent. When uncertain, treat as craft but widen the voice-guard gate — or ask the user. The standing-exemption list is in `references/pipeline.md` §4.

## How to work

**LIGHT EDIT — quick-scan path (any length):**
1. Read `references/quick-scan.md` in full — your working cue sheet for all six passes.
2. Classify genre (craft vs. art); build the voice fingerprint. Mode contracts and the standing-exemption list live in `references/pipeline.md` §3–4 — read those sections if a voice-guard call is uncertain.
3. Run the passes in order from the quick-scan cues. Open a pass's full reference file(s) only when a detect or keep-cut decision is uncertain — the full entries carry the per-rule **Examples** and **Exceptions** that calibrate judgment and gate the voice guard.
4. For texts over ~10 sentences, keep the Pass 1 ledger as a working table and process paragraph by paragraph.
5. Before returning: run the `references/pipeline.md` §7 verification checklist; then output per the contract below.

**FULL REWRITE and DIAGNOSE — full pipeline:**
1. Read `references/pipeline.md` in full.
2. Classify mode + genre (craft vs. art); build the voice fingerprint.
3. Run the passes in order. While running a pass, consult that pass's reference file(s) — each has per-rule **Detect** signals, numbered **Fix** steps, before/after **Examples**, and **Exceptions**, plus a "Quick scan list" for fast application.
4. For texts over ~10 sentences, keep the Pass 1 ledger as a working table and process paragraph by paragraph.
5. Before returning: run the §7 verification checklist; then output per the contract below.

## Output contract

- **DIAGNOSE:** the findings report (ledger + violations), most severe first. No rewrite.
- **LIGHT EDIT / FULL REWRITE:** (1) the finished text first; (2) a short "What changed" summary naming the biggest structural moves and the rules behind them; (3) any `[AUTHOR CHECK]` items as a list. Don't show pass-by-pass intermediates unless asked.
- Keep the summary proportionate: a three-sentence email needs two lines, not a table.

## Reference files

| File | Contents |
|---|---|
| `references/quick-scan.md` | Condensed detect cues for every rule, all five rule files merged — the LIGHT EDIT cue sheet |
| `references/pipeline.md` | The six passes in full, mode contracts, voice guard, worked example, verification checklist |
| `references/reader-first.md` | Reader-serving doctrine, information hierarchy, sentence-length strategy, surgery cuts, accuracy/voice verification |
| `references/structure.md` | Main-clause news, X-ray method, subordination traps, nominalizations, passives, clause parsing |
| `references/modifiers.md` | Placement, danglers, relative clauses, pronouns, parallels, manner adverbs |
| `references/concision.md` | Vague→specific words, fatty prose, metaphors, fact-cramming, attribution, semicolons |
| `references/grammar-mechanics.md` | Grammar tables, tense selection, punctuation, run-ons, iconic errors |
