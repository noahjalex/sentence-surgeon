# The Casagrande Rewrite Pipeline

A deterministic, ordered, multi-pass process for reorganizing and rewriting prose using June Casagrande's sentence-craft method. An LLM editor runs the passes in order, in one of three operating modes, with a voice guard active around every pass.

---

## 1. Rule-file map

Every principle cited below lives in exactly one skill file. The pass that applies a principle may live elsewhere; passes borrow across files freely (the pass order controls *when* a rule fires, not *where* it is documented).

| File | Rule groups |
|---|---|
| `reader-first.md` | Reader-serving doctrine (Serve the Reader, Lead With Reader Impact, No Sleaze Bait, Main Point in Main Clause); sentence-length strategy (Brevity Is a Tool, Don't Bury the Verb, No Monster Openers, Let Facts Stand Alone); Uncram (Decompose, Reorder, Rebuild); Say It Like You'd Speak It; surgery cuts (Kill Meta-Announcements, Cut Redundant Clauses and Doubled Actions, Explain Fully or Not at All, Cut Writer-Serving Modifiers, Pare-Down Modifier Test, One Time Element Per Action); Verify Accuracy and Voice |
| `structure.md` | Main clause carries the main news; Main-Clause X-Ray and rebuild (Concrete-Noun Swap, Real-Action Verb); nominalizations and buried verbs; the+gerund+of; has+noun+participle; clause-as-noun subjects; all passive-voice rules; coordination vs. subordination; conjunction traps (as/while/since/if/than); phrase/clause parsing and portability |
| `modifiers.md` | Modifier placement (nearest-word attachment, pileups, trailing scope); danglers; relative clauses (overload, restrictive commas, zero relative, that/which); pronouns and antecedents; parallels and lists (One List One Form, suspended compounds, repeated function words); manner-adverb deletion test; gerund/participle ambiguity |
| `concision.md` | Specific words over opaque ones; bold statements; he-is-a-man-who; flabby insertions; dead-weight adjectives; goldbricking adverbs; definition-level redundancy; fact-cramming (parentheses, attribution tails); failed metaphors; mushy cores; literal-meaning audit; from...to; Said Is the Default Attribution; semicolon discipline |
| `grammar-mechanics.md` | Reference tables (sentence structures, phrases, clauses, verbs, pronoun case); tense selection and consistency (all five tense rules); punctuation rules; run-ons and comma splices; iconic credibility-killing errors |

---

## 2. Why the passes run in this order

Structure comes before everything downstream because splitting, merging, and re-anchoring main clauses destroys any finer-grained work done earlier — a carefully relocated modifier is wasted effort if its sentence is about to be broken in two. But structure itself depends on knowing the point, so intent diagnosis (Pass 1) comes first of all: you cannot decide what belongs in a main clause until you have stated, per sentence, what the sentence is *for* and whom it serves. Placement and reference fixes (Pass 3) need stable sentence boundaries but must precede word-level cuts, because you cannot judge whether a modifier is fat until it is attached to the word it actually modifies. Word-level trimming and diction (Pass 4) come next, since deletion tests are only meaningful on correctly built sentences. Mechanics (Pass 5) attach punctuation and consistency policy to final wording, so they run second-to-last. The Reader read-through (Pass 6) runs last because it judges the assembled whole — rhythm, lead, honesty, accuracy, voice — which does not exist until every prior pass is done.

---

## 3. Operating modes

The pipeline runs identically through Pass 1 in all modes (Pass 1 *is* the diagnostic engine). Modes diverge from Pass 2 on.

| Mode | Contract |
|---|---|
| **DIAGNOSE** | Run all six passes read-only. Output a findings report: sentence number, principle violated (with its home file), quoted evidence, severity, and the fix *direction* (one line). No text is changed. Findings that a structural fix would moot are still reported, tagged `mooted-by-structure` so the caller can prioritize. Voice-guard judgments appear as `deliberate-device?` notes instead of exemptions. |
| **LIGHT EDIT** | Fix confirmed violations in place while preserving the author's sentence order, imagery, register, and (where possible) sentence boundaries. Pass 2 may split a sentence but never merge, reorder facts across sentences, or rebuild the lead (lead problems are flagged, not fixed). Pass 4 deletes only zero-meaning-loss fat and swaps only detection-list vague words whose referent is certain from the text. Pass 6 flags writer-serving content as `[AUTHOR CHECK]` notes rather than cutting it. Voice guard: **ties go to the author.** |
| **FULL REWRITE** | Everything is allowed in service of the Reader: reorder information across sentences (Dominoes: Decompose, Reorder, Rebuild), rebuild the lead (Lead With Reader Impact), delete whole sentences (Kill Meta-Announcements), add plain connectives ("So," "But"). Voice guard: **ties go to the Reader**, but fingerprint devices (Section 4) remain protected and every content cut is logged. |

**Invariant across all modes:** the pipeline may only redistribute information already present in the source. It never invents facts. Where "Explain Fully or Not at All" offers the *add specifics* fork, the editor either uses specifics found elsewhere in the source or emits an `[AUTHOR CHECK]` asking the author to supply them. When a full fix would require a fact the source doesn't supply — an actor for a passive, a referent for a pronoun, an effective date — the compliant terminal state in every mode is maximal repair plus an `[AUTHOR CHECK]` naming the missing fact; residue at that point is complete handling, not a violation.

---

## 4. Voice guard

The book's creed: these are "safe havens," not laws — craft rules that a writer may defy with grace. The voice guard is the mechanism that keeps the pipeline from becoming the dogma the book warns against.

**4.1 Fingerprint (built once, before Pass 1).** Record: register (formal/casual/comic), person and tense scheme, genre and its Reader expectations (a film-festival logline is not a police report), and signature devices — deliberate fragments, one-word sentences, repetition for rhythm, irony, parentheticals-as-voice, profanity policy, dialect. A device that *recurs* is presumed a choice, not an accident.

**4.2 Pre-pass gate (before applying any rule to any sentence).** Ask: is this violation plausibly deliberate craft? Signals: it is in the fingerprint; it recurs; it buys rhythm, suspense, humor, or emphasis; the genre expects it. If yes, do not apply the rule mechanically — draft both versions and keep whichever serves the Reader better (Evaluate, Don't Eradicate; Nominalization Is an Opportunity, Not an Error; Deletion-Test Every Manner Adverb). In LIGHT EDIT a tie keeps the original; in FULL REWRITE a tie takes the rewrite.

**Keeping is a decision, not a shrug.** When you keep text that matches a violation pattern on deliberate-device grounds: if the device recurs (it's in the fingerprint), a one-line changelog note suffices; if the keep rests on a single-instance judgment call (one purple image chain, one said-bookism, one stacked-relative sentence), emit `[AUTHOR CHECK: kept "..." as deliberate style — confirm]` so the author rules on intent. A "kept as deliberate device" line in the changelog is a verdict; borderline cases deserve a question. One asymmetry: recurrence exempts only *texture* devices (fragments, repetition, register, parentheticals). *Structural* keeps — upside-down subordination, stacked relative clauses, a buried lead — get the `[AUTHOR CHECK]` in LIGHT EDIT even when they recur, because accidental structure mimics deliberate structure too well to presume intent.

**4.3 Post-pass diff (after every pass).** Run Verify Accuracy and Voice After Every Rewrite (`reader-first.md`): compare against the pre-pass text for dropped facts, numbers, time markers, flipped event sequence, who-did-what changes from passive/active flips, deleted qualifiers, and flattened deliberate devices. Restore anything lost by accident; log anything removed on purpose.

**4.4 Standing exemptions (rules deliberately NOT applied when the exception fires).**
- Passives kept when they rightly downplay the doer or keep focus on the receiver (Downplay the Doer Deliberately, `structure.md`).
- Short wry parentheticals kept as voice (Keep Voice-Device Parentheses — see Unpack Fact-Stuffed Sentences exceptions, `concision.md`).
- Manner adverbs kept when they survive the deletion test — they change the verb's meaning or supply pace nothing else carries (Deletion-Test Every Manner Adverb, `modifiers.md`).
- Fragments, clefts, dislocations, and fronted phrases kept as deliberate emphasis (Emphasis by Rearrangement, `grammar-mechanics.md`); imperatives and nonfinite clauses are never "corrected" (Don't Over-Correct Imperatives, `structure.md`).
- Subordinating-conjunction openers kept when the wind-up is short and the main clause still carries the point (Main Point in Main Clause exception, `reader-first.md`).
- Long sentences kept when they sit on a foundation of short ones and the writer could have written the short version (Brevity Is a Tool — Mix Lengths by Genre; Placement Counts, `reader-first.md`).
- Sustained present-tense narration kept as a deliberate device (`grammar-mechanics.md`).
- Full-strength profanity kept as a legitimate voice choice; only reflexive oomph-seeking intensifiers die (Cut Modifiers That Serve the Writer exception, `reader-first.md`).
- Charged word choices ("stupid") that may carry author intent are never silently replaced — emit `[AUTHOR CHECK]`.

---

## 5. The passes

### Pass 1 — X-Ray & Intent Ledger

**Goal:** Build a per-sentence ledger that states each sentence's core, its point, and whether the main clause carries that point — no text changes in any mode.

**Scoping:** headings, subject lines, salutations, signatures, bulleted fragments, and other display units are not sentences — ledger them as "non-sentence unit — skip" and exempt them from sentence rules (do not "complete" a greeting or a list fragment) unless the user asks otherwise. Text inside direct quotation marks is ledgered but never rewritten (see the quote invariant in SKILL.md).

This pass operationalizes the book's core diagnostic (Main-Clause X-Ray). For **every** sentence:

1. Isolate the main subject and main verb (strip adverbials, modifiers, subordinate clauses; pair every doer with its action and find the unsubordinated pair).
2. State the sentence's point in one line ("What am I really trying to say?").
3. Check the main clause carries that point: **Whom is this sentence about? What did they do? Does the Reader care?**

**Questions asked of each sentence:**
- What is the bare main clause (subject + verb + complement head)? Read it aloud: does it say anything new, or is it a blank-is-a-blank ("this depiction is a portrait")?
- What is this sentence's single most important fact, and is it in the main clause or parked in a subordinate/participial/prepositional annex?
- Whom is it about? What did they do? Why should the Reader care — and is that answer the sentence's overt content?
- Is any content writer-serving (pity, showing off, insider references, verdict words, manufactured menace)?
- How many separable propositions does the sentence carry?

**Rule groups:** Serve the Reader Not Yourself, Lead With Reader Impact, No Sleaze Bait, Main Point in Main Clause, Uncram [detection only] (`reader-first.md`); Main-Clause X-Ray and Rebuild [detection only], Pair Doers with Actions to Find Clauses, Phrase-vs-Clause Test (`structure.md`); Strip Adverbials to Find the Core (`grammar-mechanics.md`).

**Mode behavior:** Identical in all modes; in DIAGNOSE the ledger is the top of the output report, in edit modes it drives Passes 2–6.

**Stop condition:** Every sentence has a ledger row containing (a) bare core, (b) one-line point, (c) whom/what/care answers, (d) PASS/FAIL verdict, and every FAIL cites at least one named principle. 100% coverage, no row blank.

---

### Pass 2 — Structural Rebuild

**Goal:** Give every sentence a main clause whose concrete subject and action verb carry the ledger's stated point — the sentence-architecture pass.

**Questions asked of each sentence (against its ledger row):**
- Is the main news in the main clause, or upside-down under a subordinator (after/although/while/until...) while the main clause reports a mood or a state?
- Does a subordinate chain stack vivid actions in front of an abstract subject and weak verb? Unstack: one main clause per major action.
- Is the subject an abstract noun propped up by of-phrases (Concrete-Noun Swap)? Is the main verb "is" in costume — emerges as, serves as, stands as (Real-Action Verb)? Is the real action buried in a nominalization, a "the + gerund + of," a has-noun-participle frame, or a clause-as-noun subject?
- Is the sentence a true passive (be + past participle, subject is the doee)? Draft the active twin; choose deliberately (Evaluate, Don't Eradicate) — activate squelched action, recover missing actors, keep only deliberate doer-downplaying passives.
- Is the sentence crammed (30+ words, 3+ finite clauses, subject far from verb)? Cut dispensable facts, then split; decompose–reorder–rebuild when backstory is tangled.
- Are equals coordinated and unequals subordinated? Are 'as'/'while'/'if'/'since' carrying meanings they can't bear? Are lists structurally parallel (one list, one form; stem-distribution test)?
- Is the tense the simplest that preserves meaning? Do perfect tenses have an "another" to relate to? After one anchor, do following sentences simplify?

**Rule groups:** Main Clause Carries the Main News, Unstack the Loaded Subordinate Chain, Coordinate Equals Subordinate to Encode Relationships, 'As' Means Simultaneous, Definition Traps (while/since/if/than), Main-Clause X-Ray and Rebuild (Concrete-Noun Swap, Real-Action Verb), Un-Stuff "has + noun + participle", Unstick Clause-as-Noun Subjects, all passive principles, all nominalization principles, Parse Units Before Moving Them (`structure.md`); Brevity Is a Tool, Don't Bury the Verb, Uncram (Decompose, Reorder, Rebuild), Let Facts Stand Alone, Placement Counts: No Monster Openers, Main Point in Main Clause, Kill Meta-Announcements and Statements of the Obvious (`reader-first.md`); One List One Form / Stem-Distribution Test (`modifiers.md`); tense-selection rules (`grammar-mechanics.md` §7); Collapse Empty Existentials and Clefts (`structure.md`); Collapse 'He Is a Man Who' Frames (`concision.md` — borrowed early because the frame *is* a main-clause disease).

**Mode behavior:** DIAGNOSE — report flags with fix directions. LIGHT EDIT — within-sentence surgery only: may split one sentence into two, may not merge, reorder facts across sentences, or rebuild the lead (flag instead); whole-sentence deletions are flags, not cuts. FULL REWRITE — full dominoes across the paragraph, lead rebuild, sentence deletion allowed (logged).

**Stop condition:** Re-run the Pass 1 X-ray on every rewritten sentence. Stop when a full re-scan yields zero new structural flags: no blank-is-a-blank cores; no upside-down subordination; no doer-less gerund/nominalization/clause subjects and no unjustified passives (each survivor has a logged reason); no sentence over ~30 words without a logged justification; all lists parallel.

---

### Pass 3 — Placement & Reference

**Goal:** Make every movable part sit next to what it modifies and every pointing word point at exactly one thing.

**Questions asked of each sentence:**
- Does each prepositional phrase touch the word it modifies (Readers attach to the nearest candidate)? If every reordering parks it wrong, rewrite the pileup.
- Does a fronted participle's action belong to the main-clause subject? (Un-dangle: make the true doer the subject, or convert to a subordinate clause.) Does a fronted modifier lean on a possessive that can't anchor it?
- Could an -ing word before a noun read as either gerund or participle? Insert "that" or restructure.
- Does a trailing modifier after a list cover all items or just the last (List-Modifier Scope Check)? Should a shared preposition repeat before each item? Do suspended compounds and stem modifiers fit every item?
- Relative clauses: at most one per sentence? Extra light on the noun, or smuggled backstory (move or flag for cut)? Restrictive vs. nonrestrictive comma'd by the removal test; that/which per the governing style; zero relatives dropped where they read better — but first verify the clause actually modifies a noun.
- Every third-person pronoun and possessive determiner: run the substitution test — is there exactly one candidate antecedent? Substitute the noun where two candidates compete; after naming once, trust the pronoun until a rival noun intervenes; give vague "it"/broad "that"/"which" an explicit noun; when no synonym disambiguates, repeat the noun (repetition beats chaos).
- Ambiguous 'than' comparisons: restore the elided verb. Multiple time elements: at most one anchored time element per action; pare-down test on suspect modifiers (read the literal claim of subject + modifier + verb).

**Rule groups:** Nearest-Word Attachment, Rewrite the Pileup, List-Modifier Scope Check, Un-dangle the Participle, Possessives Can't Anchor Modifiers, Split the Gerund-Participle Twins, Repeat the Shared Function Word, Complete Suspended Compounds, Elide Only What the Reader Instantly Restores, all relative-clause principles, all pronoun principles (`modifiers.md`); Parse Units Before Moving Them (nesting-doll phrases move whole; preposition + object travel as a team; pronoun-swap object test), Restore the Verb After Ambiguous 'Than' (`structure.md`); One Time Element Per Action, Pare-Down Modifier Test (`reader-first.md`).

**Mode behavior:** DIAGNOSE — report. LIGHT EDIT — fix in place; a dangler may force a subject change but not a fact reorder. FULL REWRITE — same, plus may re-split a sentence when no placement fix works inside it.

**Stop condition:** A full re-scan finds zero placement/reference flags: every fronted participle's doer is its subject; every PP is adjacent to its target or the sentence was rewritten; each sentence carries at most one relative clause, comma'd correctly; every third-person pronoun passes the substitution test with exactly one candidate; every moved unit moved whole (headword plus everything hinged to it).

---

### Pass 4 — Fat & Word Choice

**Goal:** Make every surviving word carry information the Reader needs — cut fat, then replace vague and false words with specific true ones.

**Questions asked of each sentence:**
- Deletion test every modifier: if cutting it changes nothing, cut it. Empty modifiers entailed by their noun or verb? Bare intensifiers (very, really, totally)? Redundant manner adverbs (previously + past tense)? Dead-weight verdict adjectives (renowned, brilliant)? Stacked evaluative adjectives — vote the weakest off the island. (Scope check first: only *manner/degree* adverbs are targets; never cut time, place, linking, or sentence adverbs on this rule's authority.)
- Coordinated near-synonyms and doubled actions ("cancel the membership and end all involvement") — does one action cover the bases?
- Fatty strings: due to the fact that → because; in terms of; for a period of; the question as to whether; "In addition to..., also"; hedged meta-frames ("one of the more remarkable aspects of X is the fact that...") → state the fact (Make Bold Statements).
- Vague words: impact/affect/utilize/things/items/person/area/got/went → the specific, direction-encoding word (relieves, jams, cuts). Hedges ("something," "some kind of") → be specific or be silent. Half-explanations ("we're too different") → explain fully or delete.
- Literal-truth audit: does the sentence assert nonsense when read literally? Failed metaphors that raise unanswerable questions → cash them in for plain statements. Mushy subject-verb cores ("a monotone shirred") → concrete noun + plain verb. Invented compound modifiers ("failure-doomed") → the idiomatic unpacking. Part-plus-whole comparisons → name the whole.
- Would the author ever *say* this aloud ("purchased and thirstily consumed a cola beverage")? Restore the watercooler version, then add back only the precision the genre requires.
- Attribution: said is the default; showy attribution verbs and participle-stuffed attribution tails get cut.

**Rule groups:** all fatty-prose principles (Cut Dead-Weight Adjectives, Delete Empty and Redundant Adverbs, Chop Flabby Insertions and Segues, Recast 'The Fact That', Discipline From...To Constructions, Make Bold Statements, Hunt Definition-Level Redundancy) and all word-choice principles (Nonsense-Assertion Audit, Release the Runaway Phrase, Delete Empty Modifiers, Name the Whole Not the Part, Cash In Failed Metaphors, Anchor the Mushy Core, No Invented Compound Modifiers, Specific Words Over Opaque Ones, Be Specific or Be Silent) (`concision.md`); Deletion-Test Every Manner Adverb (classify by function; only manner/degree in scope), Show Results Don't Tell Manner (`modifiers.md`); Say It Like You'd Speak It, Cut Redundant Clauses and Doubled Actions, Explain Fully or Not at All, Cut Modifiers That Serve the Writer (`reader-first.md`); Said Is the Default Attribution, Quoted Words Are Verbatim, Unpack Fact-Stuffed Sentences (attribution tails) (`concision.md`).

**Mode behavior:** DIAGNOSE — report each fat token and vague word with its proposed replacement. LIGHT EDIT — delete only zero-meaning-loss fat; swap vague words only when the specific referent is certain from the source text; adverb cuts require a passed deletion test. FULL REWRITE — full diction rebuild; the "explain fully" fork may pull specifics from anywhere in the source, else `[AUTHOR CHECK]`.

**Stop condition:** No detection-list token remains without a logged justification (deletion-test pass or voice-guard exemption); every surviving modifier changes meaning when deleted; a literal-meaning re-read of every sentence finds zero nonsense assertions; no unresolved hedge words.

---

### Pass 5 — Mechanics & Consistency

**Goal:** Make punctuation, agreement, tense consistency, and the credibility-killing details correct and internally consistent — on the now-final wording.

**Questions asked of each sentence:**
- One serial-comma policy document-wide? Junction commas after long introductions and before clause-joining conjunctions? Paired commas opened *and* closed; restrictive elements bare, nonrestrictive set off (removal test)? And-test on stacked modifiers?
- Semicolons: ubercomma use only; clause-joining semicolons → periods (LIGHT EDIT: a deliberate clause-joining semicolon is a voice choice — keep it unless the page is semicolon-riddled); semicolon-stuffed lists broken up. Parentheses: fact-cramming parentheticals (>5 words or containing numerals/specs) unpacked into sentences; voice-device asides ≤5 words survive.
- Run-ons and comma splices joined properly. One terminal mark only. American quote-mark order. Hyphenate preposed compounds that could misread; never after -ly.
- Apostrophes: possessives placed correctly, never pluralizing. Iconic-error string scan: its/it's, there/their/they're, could of, have went, then/than, affect/effect, lets/let's, phase/faze, led/lead...
- Object pronouns after prepositions; adjectives (not -ly adverbs) after copular verbs; modern subjunctive after demand/suggestion and contrary-to-fact; negation/questions formed on the operator.
- Tense consistency: every verb about one timeframe in one tense; still-true facts in that-clauses stay present (logged as deliberate); usage and style questions resolved by the governing stylebook, not remembered "rules."

**Rule groups:** all punct-errors principles (Serial-Comma Consistency, And-Test, Junction Commas, Paired Commas Signal Meaning, Apostrophes Never Pluralize, American Quote-Mark Order, One Terminal Mark Only, Hyphenate Preposed Compounds, Kill the Iconic Errors), semicolon and parentheses discipline (Semicolons Earn Their Keep; Unpack Fact-Stuffed Sentences, incl. Keep Voice-Device Parentheses — `concision.md`), subject–verb agreement (`grammar-mechanics.md` §5a), Look Up Usage and Style, Operator Rules, Adjectives After Copular Verbs, Object Pronouns After Prepositions, Apostrophe Placement, Modern Subjunctive, Indirect-Object Swap, Fix Run-Ons and Comma Splices, Remember the When (No Accidental Tense Shifts), Still-True Facts Stay Present, Anchor Once Then Simplify (`grammar-mechanics.md`).

**Mode behavior:** Near-identical fixes in LIGHT EDIT and FULL REWRITE — true errors (iconic errors, agreement, unclosed comma pairs, splices) never threaten voice; style-policy items follow the document's existing dominant policy. The exception is punctuation-as-voice: deliberate clause-joining semicolons and voice parentheticals get kept in LIGHT EDIT (flag if habitual). DIAGNOSE — report only.

**Stop condition:** One full mechanical scan (comma policy, comma pairs, semicolons, parentheses, apostrophes, quotes, terminal marks, hyphens, iconic-error strings, tense-shift check) returns zero errors.

---

### Pass 6 — Reader Read-Through

**Goal:** Read the assembled text once as the Reader — verify every sentence earns its place, the whole is honest and accurate, and the author still sounds like the author.

**Questions asked (of the whole, then of each sentence):**
- Whom is this sentence about? What did they do? Does the Reader care? (Final application of the Pass 1 triad, now to the finished text.)
- Does the piece lead with Reader impact — and *without* sleaze (real stakes, no withheld referents, no manufactured menace)?
- Is any surviving content writer-serving: unexplained insider references, verdict words, self-praise the facts contradict, backstory the Reader didn't ask for? Cut or reframe (FULL REWRITE) or flag `[AUTHOR CHECK]` (LIGHT EDIT).
- Rhythm: mostly short sentences with mixed lengths — no droning runs of identical short ones, no monster opener?
- Accuracy diff against the ORIGINAL (not just the previous pass): every fact, number, time marker, event sequence, and who-did-what preserved? Every deliberate deletion logged?
- Voice: does the fingerprint survive — register, devices, humor? Would the author disown any sentence?

**Rule groups:** Serve the Reader Not Yourself, Lead With Reader Impact, No Sleaze Bait, Kill Meta-Announcements, Explain Fully or Not at All, Verify Accuracy and Voice After Every Rewrite, Brevity Is a Tool — Mix Lengths by Genre, Placement Counts: No Monster Openers (`reader-first.md`); Extra Light Not Backstory (`modifiers.md`); Keep Voice-Device Parentheses (`concision.md`); Emphasis by Rearrangement [as exemption authority] (`grammar-mechanics.md`).

**Mode behavior:** DIAGNOSE — report Reader-level findings. LIGHT EDIT — content problems become `[AUTHOR CHECK]` flags; only accuracy restorations are applied. FULL REWRITE — cut or reframe writer-serving content; log every content cut for the author; when a named source or attribution is cut, state in the change summary where their content went and flag `[AUTHOR CHECK]` for sign-off.

**Re-scan requirement:** any sentence rewritten or reframed in this pass must re-run the Pass 3 (placement/reference) and Pass 5 (mechanics) scans before return — text produced last must not ship unverified.

**Stop condition:** For every sentence the editor can answer whom/what/why-care without hesitation; the accuracy diff is clean (or every difference is a logged deliberate cut); every fingerprint item survives or has a logged justification; the verification checklist (Section 7) is all-green.

---

## 6. Worked example (FULL REWRITE mode)

### Original (planted violations noted)

> After hackers broke into the payroll system, stole the tax records of 4,000 employees, and posted them for sale online, the company's IT director was concerned. Working around the clock to contain the breach, the utilization of an outside security firm was seen by management as the best option due to the fact that internal resources were limited. The CEO met with the IT director and told him that his response had negatively impacted things. In terms of employees, an announcement was made that there would be the offering of credit monitoring for a period of twelve months. The company, which was founded in 1987 and which has offices in three states, is a firm that takes security very seriously.

Planted: upside-down subordination (S1); dangler (S2, "Working around the clock..." modifies "the utilization"); nominalizations (S2 "the utilization of," S4 "the offering of"); passives (S2 "was seen by management," S4 "an announcement was made"); vague words (S3 "negatively impacted things," S2 "internal resources were limited"); fatty phrases ("due to the fact that," "in terms of," "for a period of"); unclear pronouns (S3 "his," bonus S1 "them"). Bonus: double relative-clause backstory, "is a firm that" frame, and "very" (S5).

**Voice fingerprint:** neutral corporate-news register, third person, past-tense report, no signature devices. Few exemptions expected.

### After Pass 1 — X-Ray & Intent Ledger (no text change)

| # | Bare core | Point in one line | Whom / what / does the Reader care? | Verdict |
|---|---|---|---|---|
| 1 | *director was concerned* | Hackers stole 4,000 employees' tax records and put them up for sale | About hackers who stole records; main clause is about a man's mood | FAIL — Main Clause Carries the Main News; Unstack the Loaded Subordinate Chain (`structure.md`) |
| 2 | *utilization was seen* | The company hired an outside security firm because its own team couldn't cope | Abstract subject + passive + dangler; Reader can't see who did what | FAIL — Main-Clause X-Ray, True-Passive Test (`structure.md`); Un-dangle the Participle (`modifiers.md`) |
| 3 | *CEO met and told* | The CEO blamed the response for worsening things — whose response, worse how? | "his" ambiguous; "negatively impacted things" not checkable | FAIL — Substitute the Noun (`modifiers.md`); Specific Words Over Opaque Ones (`concision.md`) |
| 4 | *announcement was made* | Employees get twelve months of credit monitoring | Doer missing; action buried in "the offering of"; Reader impact buried | FAIL — Recover the Missing Actor, Kill the-Gerund-of (`structure.md`); Lead With Reader Impact (`reader-first.md`) |
| 5 | *company is a firm* | Unclear — self-praise plus founding trivia | Blank-is-a-blank; backstory and PR serve the writer | FAIL — Main-Clause X-Ray (`structure.md`); Serve the Reader (`reader-first.md`); Extra Light Not Backstory (`modifiers.md`) |

### After Pass 2 — Structural Rebuild

Rules applied: S1 — Unstack the Loaded Subordinate Chain + Main Clause Carries the Main News (`structure.md`); the anticlimactic "director was concerned" deleted as a statement of the obvious (Kill Meta-Announcements, `reader-first.md`; FULL REWRITE only, logged). S2 — Main-Clause X-Ray, Concrete-Noun Swap, Real-Action Verb, Activate Squelched Action, Recover the Missing Actor (`structure.md`): the actor comes from the by-phrase — but the source says only that the firm "was seen by management as the best option," so the rebuild asserts management's judgment, not a hiring the source never states (no-new-facts invariant; logged [AUTHOR CHECK: was the outside firm actually engaged?]). The rebuild hands "worked around the clock" to its true doer, dissolving the dangler (verified next pass). S3 — passes the X-ray (concrete subject, action verbs); untouched. S4 — Recover the Missing Actor, Kill the-Gerund-of ("the offering of" → "offer"), Exploit Portability (folds "in terms of employees" into the verb's object) (`structure.md`). S5 — Don't Bury the Verb (two relative clauses sat between subject and verb; split out) (`reader-first.md`); Collapse 'He Is a Man Who' Frames ("is a firm that takes" → "takes") (`concision.md`). Survivor passive logged: "was founded" (S5) — receiver-topic backstory, kept pending the Pass 6 relevance review.

> Hackers broke into the payroll system, stole the tax records of 4,000 employees, and posted them for sale online. The internal team worked around the clock to contain the breach, but management saw an outside security firm as the best option due to the fact that internal resources were limited. The CEO met with the IT director and told him that his response had negatively impacted things. The company announced that it would offer employees credit monitoring for a period of twelve months. The company was founded in 1987 and has offices in three states. It takes security very seriously.

### After Pass 3 — Placement & Reference

Rules applied: S1 — "posted them" has two plural candidates (records, employees); Substitute the Noun + Audit Third-Person Pronouns (`modifiers.md`) → "posted the records." S2 — Un-dangle the Participle (`modifiers.md`): verified resolved by the Pass 2 rebuild; no edit. S3 — "his" has two candidates (CEO, director); Substitute the Noun + Repetition Beats Chaos (`modifiers.md`) → "the director's response"; "him" is safe (Fix Once, Then Trust the Pronoun). S5/S6 — "It" points uniquely to "The company": passes; Extra Light Not Backstory (`modifiers.md`) flags the founding/offices facts as backstory — relevance call deferred to Pass 6.

> Hackers broke into the payroll system, stole the tax records of 4,000 employees, and posted the records for sale online. The internal team worked around the clock to contain the breach, but management saw an outside security firm as the best option due to the fact that internal resources were limited. The CEO met with the IT director and told him that the director's response had negatively impacted things. The company announced that it would offer employees credit monitoring for a period of twelve months. The company was founded in 1987 and has offices in three states. It takes security very seriously.

### After Pass 4 — Fat & Word Choice

Rules applied: S2 — Chop Flabby Insertions (`concision.md`): "due to the fact that" → "because." "Internal resources were limited" stays as is — the specific referent (staff? budget? tools?) is unrecoverable from the source, and narrowing it would invent a fact (Specific Words Over Opaque Ones defers to the no-new-facts invariant). S3 — Cut Redundant Clauses and Doubled Actions (`reader-first.md`): "met with the IT director and told him" → "told the IT director" (the telling implies the meeting); Specific Words Over Opaque Ones (`concision.md`): "had negatively impacted things" → "had made things worse" (plain verb; the referent of "things" is unrecoverable — logged [AUTHOR CHECK: worsened what, specifically?]). S4 — Chop Flabby Insertions (`concision.md`): "credit monitoring for a period of twelve months" → "twelve months of credit monitoring." S6 — Delete Empty and Redundant Adverbs (`concision.md`) + Cut Modifiers That Serve the Writer (`reader-first.md`): "very" deleted (deletion test: no meaning change).

> Hackers broke into the payroll system, stole the tax records of 4,000 employees, and posted the records for sale online. The internal team worked around the clock to contain the breach, but management saw an outside security firm as the best option because internal resources were limited. The CEO told the IT director that the director's response had made things worse. The company announced that it would offer employees twelve months of credit monitoring. The company was founded in 1987 and has offices in three states. It takes security seriously.

### After Pass 5 — Mechanics & Consistency

Rules applied: Junction Commas (`grammar-mechanics.md`) — comma before "but" joining independent clauses: present. Serial-Comma Consistency (`grammar-mechanics.md`) — the one list uses the serial comma; policy consistent. Still-True Facts Stay Present (`grammar-mechanics.md`) — considered for "would offer" and rejected: the rule covers timeless truths, not reported commitments, and whether the offer still stands is unknowable from the source; "would offer" stays (logged [AUTHOR CHECK: is the credit-monitoring offer still current?]). Iconic-error and apostrophe scans: clean.

> Hackers broke into the payroll system, stole the tax records of 4,000 employees, and posted the records for sale online. The internal team worked around the clock to contain the breach, but management saw an outside security firm as the best option because internal resources were limited. The CEO told the IT director that the director's response had made things worse. The company announced that it would offer employees twelve months of credit monitoring. The company was founded in 1987 and has offices in three states. It takes security seriously.

### After Pass 6 — Reader Read-Through

Rules applied: Lead With Reader Impact + No Sleaze Bait (`reader-first.md`) — the news leads, stakes are real, nothing withheld: lead passes. S4 — the *announcement* is process; the Reader cares what employees get (Lead With Reader Impact) → "Employees will get twelve months of credit monitoring, the company announced" (Reader impact leads; the trailing attribution keeps the claim anchored to its source instead of asserted flat, so the reported modality survives; sentence re-scanned per the Pass 6 re-scan requirement). S5 — founding date and office count are backstory with no bearing on the breach (Extra Light Not Backstory, `modifiers.md`; Serve the Reader Not Yourself, `reader-first.md`) → cut, logged `[AUTHOR CHECK]`. S6 — "It takes security seriously" is writer-serving self-praise the surrounding facts contradict, with no specifics behind it (Explain Fully or Not at All + Cut Modifiers That Serve the Writer, `reader-first.md`; Make Bold Statements, `concision.md`) → cut ("either explain or don't — don't half-ass it"), logged `[AUTHOR CHECK]`. Accuracy diff (Verify Accuracy and Voice, `reader-first.md`): all surviving facts, numbers, sequence, and time elements match the original; two content cuts and five [AUTHOR CHECK] items logged. Rhythm (Brevity Is a Tool — Mix Lengths): sentence lengths 20/27/14/11 words — varied, no drone.

### Final version

> Hackers broke into the payroll system, stole the tax records of 4,000 employees, and posted the records for sale online. The internal team worked around the clock to contain the breach, but management saw an outside security firm as the best option because internal resources were limited. The CEO told the IT director that the director's response had made things worse. Employees will get twelve months of credit monitoring, the company announced.

### What changed (3 lines)

1. The buried news — hackers stole and posted 4,000 employees' tax records — moved from a subordinate wind-up into the lead main clause, and every sentence now pairs a concrete actor with an action verb.
2. Two passives, two nominalizations, one dangler, and two ambiguous pronouns dissolved by naming doers and referents outright; fat ("due to the fact that," "in terms of," "for a period of," "very") cut; corporate vagueness ("negatively impacted things") made plain where the source supports it and [AUTHOR CHECK]-flagged where it doesn't.
3. 120 words became 72; the only casualties were writer-serving content — founding trivia and self-praise the facts contradicted — both logged for author review.

---

## 7. Verification checklist (run before returning any rewrite)

Every item is binary. Any NO blocks return — with two mode-aware escapes: (1) in LIGHT EDIT, an item whose fix the mode contract forbids (merging sentences to fix drone, reordering facts to fix a lead, cutting content) passes when the problem is flagged `[AUTHOR CHECK]` instead of fixed; (2) any item passes on a logged voice-guard exemption.

- [ ] Every sentence's bare main subject + main verb states new information (no blank-is-a-blank cores).
- [ ] Every sentence's most newsworthy fact sits in a main clause, not a subordinate one (or the exception is logged).
- [ ] Every sentence answers: whom is it about, what did they do, why does the Reader care.
- [ ] No sentence exceeds ~30 words without a logged justification; sentence lengths vary (no drone, no monster opener).
- [ ] No "the + gerund + of" pattern remains; every surviving nominalization won its comparison test.
- [ ] Every surviving passive has a logged deliberate reason (topic focus or doer-downplay).
- [ ] Every fronted participle or modifier attaches to the subject that performs it; every prepositional phrase touches its target.
- [ ] At most one relative clause per sentence, each comma'd correctly by the removal test.
- [ ] Every third-person pronoun and possessive determiner passes the substitution test with exactly one candidate antecedent.
- [ ] All lists pass One List One Form and the stem-distribution test.
- [ ] Every surviving modifier fails the deletion test (i.e., cutting it would change meaning); every surviving manner adverb is logged as earning its place.
- [ ] No detection-list fat token or vague word remains unjustified (due to the fact that, in terms of, very, really, thing(s), impact/affect as vague verbs, some kind of...).
- [ ] No sentence asserts literal nonsense; no failed metaphor or invented compound survives.
- [ ] Tense scheme is consistent per timeframe; every complex tense has its "another"; still-true presents are logged.
- [ ] Mechanics scan clean: serial-comma policy consistent; junction and paired commas correct; semicolons/parentheses per rules; apostrophes never pluralize; quote-mark order correct; one terminal mark; hyphens checked; zero iconic errors.
- [ ] Accuracy diff against the original is clean: no fact, number, time marker, event sequence, or who-did-what changed; every deliberate content cut is logged.
- [ ] Voice fingerprint intact: every deliberate device survives or has a logged exemption decision; all `[AUTHOR CHECK]` items are listed in the output.
- [ ] Mode contract respected (DIAGNOSE: zero text changes; LIGHT EDIT: no cross-sentence reorganization, no lead rebuild, no content cuts; FULL REWRITE: all cuts and reorderings logged).
- [ ] No fact appears in the rewrite that is absent from the source.
