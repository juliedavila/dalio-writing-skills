---
name: dalio-technical-docs
description: Write or review technical docs (READMEs, paved-road guides, runbooks, reference, design docs, security findings, advisories, agent context files), or give engineering design feedback on a design doc, RFC, ADR or threat model, in Dalio's layered style held to evidence.
---

# Dalio-style technical documentation

Produces technical documents that a reader can 1) identify, 2) navigate, 3) understand, 4) act on correctly and 5) trust. It keeps Dalio's discipline (levels, nutshell first, mechanism before cases, facts separated from judgments, numbers over adjectives, named owners) and adds what the documentation research says Dalio's format lacks: task-named headings, numbered procedures, runnable examples, mechanical verification, and testing with real readers.

**Precedence.** Where Dalio style and the documentation evidence conflict, the evidence wins on structure, headings and verification. Dalio governs reasoning and voice. For decision memos, strategy docs and postmortems, use the sibling skills (`dalio-decision-memo`, `dalio-strategy-analysis`, `dalio-diagnosis-principles`) and apply only this skill's verification and security rules on top.

## What the evidence says (why the rules exist)

- Docs fail mostly on content, not style. The top problems are ambiguity, incompleteness, incorrectness and staleness (Uddin & Robillard 2015; Aghajani et al. 2019/2020).
- Readers scan. They read about 20–28% of a page, and heading and link wording drives where they go (Weinreich et al.; information foraging research).
- Task-oriented, explicit steps beat brevity for its own sake. Minimalist manuals cut learning time ~40% (Carroll et al. 1987; van der Meij & Lazonder 1993).
- Complete, task-like examples carry the most weight, especially for security. Curated secure examples gave 66% fewer vulnerabilities (Mindermann & Wagner 2018). Docs alone left ~20% of developers believing insecure code was secure (Acar et al. 2017).
- Over-signaling backfires. Combined cues had g = −0.64 in a 2026 meta-analysis.
- Novices need explicit paths, and experts are slowed by them (expertise reversal effect).
- Putting the conclusion first helps only if it is restated at the end (van der Bruggen & Maat 2025).
- LLM drafts fail on hallucinated names, especially for rare or internal APIs (~39% valid calls), on verbosity, and on silent omissions in rewrites.
- Expert and AI reviewers catch fewer than 15% of the problems real readers hit (Lentz & de Jong 1997). Review is triage. Reader testing decides.

Not supported, so don't enforce: readability-grade targets, banning the passive voice, designing for the F-pattern, "shorter is always better," "conclusion first always helps."

## House style (applies to every doc type)

1. **Levels = audience layers.** Above the line: what it is, why it exists, the main path. Below the line: reference, edge cases, internals. Detail appears only under the point it serves. A novice should succeed above the line. An expert should be able to skip straight to the reference.
2. **Nutshell first, restated last.** Every doc opens with 1–3 sentences: what this is, who it's for, what they can do with it, and its status. Explanations and arguments restate the conclusion at the end ("In other words, ..." once).
3. **Headings name the reader's task or object, front-loaded.** "Rotate the signing key," not "Procedure." "How token exchange works," not "How the machine works." Dalio's section names are scaffolding. Rename them to their content. A reader scanning only the headings should see the doc's structure.
4. **Register split.** Mechanics and verified facts are plain declaratives ("The pipeline blocks merges on critical findings."). Judgments are first person, with a confidence and a time window. Probabilities are words plus numbers ("likely, ~70%, by Q3"). Never let an unverified claim read as fact. "This is secure" or "no impact" needs evidence or must be rewritten as a judgment.
5. **Numbers over adjectives.** Replace "significant," "critical," "fast," "large" with a number, range, threshold or named comparison. Severity words require the underlying score.
6. **Format follows the reader's task.** Numbered steps for procedures. Tables for lookup. Code blocks for anything typed. Prose for reasoning and causal chains ("because," "which means"). Bullets only for true enumerations. A bullet with a "because" in it becomes prose.
7. **Sparse emphasis.** At most one bold thesis sentence per section, plus bold only for the UI labels a reader must click. No runs of one-line bullets. No decorative headings.
8. **Define every term once.** That includes internal acronyms and coined terms. Link rather than re-explain.
9. **Name owners.** Teams and roles instead of "we/they." Every doc that can go stale shows an owner and a last-verified date or version.
10. **Plain words, tight.** Cut restated context, stacked hedges, promotional tone, inflated significance and vague trailing "-ing" clauses. Prefer verbs to nominalizations ("configure," not "perform configuration of").

## Workflow

### Draft mode
1. **Settle the reader and the task.** Before writing, state the doc type, the primary reader (new user, contributor, operator/on-call, security reviewer, executive, coding agent), their assumed expertise, and their top 1–3 tasks. If you can't infer these from the source and context, ask.
2. **Ground in source.** Read the actual code, config, schema, diff, ticket or finding. Never invent behavior, flags, defaults, limits or versions. Unknowns become visible `TODO(owner): question` lines, not guesses.
3. **Draft with the doc-type structure below.**
4. **Verify mechanically.**
   - Search the source for every identifier (function, flag, env var, config key, endpoint, path, command, version) and list any that aren't found.
   - Run every example and command where it's safe to, and report the result. Mark the ones you couldn't run as untested.
   - Check that every link resolves.
   - For a rewrite or simplification, diff it against the source. List any precondition, caveat or number that was dropped, and any claim that was added.
5. **Return** the draft, the verification log, and open TODOs. Don't self-grade in the same pass. Recommend running Review mode in a fresh context.

### Review mode
Run the checklist at the end. Answer each item YES / NO / N/A with a quoted span or tool output as evidence. No quote, no YES. No overall score. Output fixes ranked by impact: content problems (wrong, missing, stale, ambiguous) before presentation problems. Ignore length as a quality signal. State plainly that this review is triage, then produce the reader-test kit.

### Design feedback mode
Use this mode when you are reviewing someone else's design (design doc, RFC, ADR, architecture diagram, threat model) and writing feedback to its author. Review mode judges the *document*. This mode judges the *design*, and then the feedback has to be written so the author acts on it.

**Evidence behind this mode.**
- About 40% of CVEs traced in three large codebases came from missing or wrong architectural security tactics. The leading ones were input validation, authorization, exposure limits and actor identification. Crypto and authentication were far behind (Santos et al. 2017).
- When designers were prompted to state the issue, the options and the reason for their choice, design quality rose (9.1 vs 7.1/12), most for designers with under 5 years' experience. Designers' confidence did not track quality (Tang et al. 2008).
- Authors test their designs against scenarios the design already handles, so the reviewer should supply the scenarios (Lassing et al. 2002).
- Generic bias awareness doesn't debias anyone; specific questions are the better bet (Mohanani et al.).
- STRIDE output was only ~40% precise. The false positives were bare category labels with no assumptions. Repudiation and elevation of privilege were under-found (Mbaka & Tuma 2022).
- About a third of code review comments are not useful. Useful ones flag defects, edge cases and specifics. Comments naming concrete elements are more useful (Bosu et al. 2015; Rahman et al. 2017).
- Feedback got implemented more when it included the location, a summary and a concrete fix. Long explanations of the problem *lowered* understanding (Nelson & Schunn 2009, correlational).
- Destructive criticism is either vague or inconsiderate, and it drives people away (Gunawardena et al. 2022). Feedback aimed at the person can lower performance (Kluger & DeNisi 1996).
- For AI-drafted comments, precision beats coverage. Google's AutoCommenter went from 54% to over 80% useful by filtering low-confidence, off-target and multi-topic comments, and "a few negative experiences erode trust." One company's AI reviewer raised PR closure time ~40% (Cihan et al. 2025).
- Severity labels (Blocking / Nit, etc.) are practitioner convention (Google eng-practices, Conventional Comments), with no outcome studies.
- No study shows that design reviews themselves reduce defects. The review process is practitioner consensus.

**Steps.**
1. **Restate the design.** In 2–3 sentences, say what's proposed, the goal and the non-goals. If you can't, the document's clarity is your first finding.
2. **Size the review.** If the design has no real trade-offs, say so and keep the feedback short. Review overhead is a cost.
3. **Probe the reasoning.** For each major decision, check that the issue, the options considered and the reason this one won are stated. Ask for at least one genuinely different alternative (including do-nothing, buy, or the existing paved road) when none was considered.
4. **Supply your own scenarios.** Run a premortem: "It's six months later and this failed, or was breached. What happened?" Write 2–4 failure, abuse and load scenarios the author didn't cover, and walk the design through them.
5. **Check security at the seams.** Identify the trust boundaries and data flows. At every boundary, check in this order:
   - who or what is identified;
   - where authorization is enforced;
   - where input is validated;
   - what exposure is limited;
   - how secrets are handled;
   - what is logged for audit (repudiation);
   - privilege-escalation paths;
   - whether data and control instructions are separated.
   Only after these, check crypto choices. Every threat names the asset, the boundary, the attacker capability and the assumption. Rank the threats; don't list categories.
6. **Agentic and LLM components** (OWASP Top 10 for Agentic Applications). Check these, which are mostly orchestration-layer risks:
   - untrusted content able to reach instructions (goal hijack, prompt injection);
   - tool permissions scoped to least privilege;
   - agent identity and delegated credentials;
   - human approval before irreversible actions;
   - inter-agent and MCP/plugin trust and supply chain;
   - memory or context poisoning;
   - cascading failure and kill switches.
7. **Operability.** Check failure modes and blast radius, rollback, observability, capacity, ownership after launch, migration, cost, and regulatory or data-classification constraints.
8. **Write the feedback** in the format below. Then delete any comment you wouldn't bet survives the author's scrutiny, or turn it into a genuine question.

**Feedback format.**
- **Summary first:**
  - **Verdict:** Approve / Approve with changes / Rework, with your confidence ("I'm ~80% confident this is sound once #1 is fixed.").
  - **What works:** one or two specific strengths. These must be specific, not padding.
  - **Top concerns:** the 1–3 that matter, ranked.
  - **Not reviewed:** what you didn't look at.
- **Comments.** Each is one block:
  `[Label] Location: Observation (fact). Concern (judgment + confidence). Why: one line on impact. Suggest: concrete change or alternative. Done when: how we'll know it's resolved.`
  - **Labels:** Blocking / Should / Consider / Question / Nit / FYI.
  - **Blocking** must name the specific failure or attack scenario it prevents.
  - **Question** is only for genuine unknowns. If you have a concern, state it.
  - Keep "Why" to one line. Rationale beyond that goes behind a link.
- **Limits.** About 10 comments at most. Batch or drop the Nits, and move anything else to an appendix.
- **Tone.** Address the design, never the author ("this flow lets...", not "you forgot..."). Never be vague when negative: every criticism carries a location and a suggestion.
- **Disagreement.** When your view conflicts with the author's domain expertise, say so. Note relative believability (who has done this ≥3 times), and propose how to settle it: a spike, data, or a named decider.
- **Closure.** Say who decides, the next step, and whether a re-review is needed.

### Reader-test kit
For the doc (or the 3 most important docs in a set), write 5–8 open, non-leading scenario questions a target reader must answer or do using only the doc. Give the expected answer and the section it lives in. Pass bar: ≥90% of readers find the answer, and ≥90% of those get it right, per question. Plan ≥2 rounds of 5–10 real target readers, revising between rounds. A five-second "what is this and who is it for?" test checks the nutshell.

## Doc-type structures

### A. README / paved-road guide
1. **Nutshell:** what it is, who it's for, why it exists, and its status (supported / beta / deprecated, which versions).
2. **Quickstart:** a complete, runnable, *secure-by-default* example that looks like a real use, not a toy. Prerequisites go before it, and the expected output after it.
3. **Why this is the right path:** a short machine paragraph. Name the atomic unit (a request, a build, a secret), the cause-effect chain, and what the paved road handles for you. Readers trust what looks most thorough, so explain the secure choice.
4. **What the guardrail enforces, and what it doesn't:** two short lists.
5. **Common tasks:** task-named headings, each a how-to (structure B).
6. **Reference** (below the line): config, flags, API. Tables for lookup.
7. **Owner, support channel, last verified.** Contributor info and SECURITY.md are linked.

### B. How-to / runbook
1. **Nutshell:** the goal, when to use this, and when *not* to (with a link to the right doc).
2. **Before you start:** permissions, access, tools, versions, and a blast-radius note.
3. **Steps:** numbered, one action each. Exact environment-specific commands in code blocks. After each critical step, the expected result. Recovery info sits *at the step where the failure occurs*.
4. **Decision points** as explicit if/then. **Stop and escalate** conditions with a named escalation target. No "investigate as appropriate."
5. **Verify it worked:** an observable check.
6. **Rollback.**
7. **Owner, last exercised date.**

### C. Reference
A one-line nutshell and the version it applies to, then tables (name | type | default | description | example). Dalio's narrative structure is off here. Precision, completeness and lookup speed win. Every entry is verified against source.

### D. Explanation / design doc
1. **Nutshell:** the design in one paragraph, plus the main trade-off.
2. **How it works:** build from the atomic unit, add layers one at a time, name the one core mechanic, and end with 3–5 numbered rules of thumb. Mark any illustrative diagram "(CONCEPTUAL)."
3. **Alternatives considered** with the reason each was rejected. If a decision is being requested, switch to `dalio-decision-memo`.
4. **Risks and assumptions,** with judgments in first person and confidence attached.
5. **What would change this design:** named indicators or thresholds.
6. **Restated conclusion.**

### E. Security finding / pen-test result / scanner message
Treat every finding as an action request. A finding nobody acts on counts as a false positive, even if the bug is real.
- **Title:** class + location + impact, front-loaded ("SQL injection in `/accounts/search` exposes all customer records").
- **Severity:** labeled CVSS version and metric group (e.g., "CVSS-BTE 4.0: 8.7"), with the threat and environmental assumptions stated. Never a bare number.
- **Where:** repo, file, line, endpoint, affected versions.
- **Why it matters:** one sentence in business terms.
- **Attacker preconditions and attack vector.**
- **Reproduce:** exact steps or a request.
- **Confirm it's a true positive:** what the developer checks.
- **Fix:** a concrete patch or diff where possible, plus the secure pattern to reuse.
- **Case vs. machine:** fix this instance, and name the guardrail or paved-road change that prevents the class.
- **Link** for depth (CWE, internal standard).
- For a set of findings, open with a nutshell and **rank the asks**: name the one or two fixes that matter most. Don't present 40 equal items.

### F. Advisory / "are we affected?" statement
Nutshell (affected or not, and what to do), VEX status (`not_affected` / `affected` / `fixed` / `under_investigation`), a justification for `not_affected`, an action statement for `affected`, exact components and versions, CVE/CWE references, root cause, attack vector, labeled CVSS, owner, as-of date, and the next update time.

### G. Executive / board security brief
Use `dalio-decision-memo` or `dalio-strategy-analysis`, plus these rules:
- Put the decision or ask first.
- State impact in business or financial terms.
- Write probabilities as words plus numbers.
- State assumptions, benchmarks and what would change the recommendation.
- Don't use a risk heat map as the primary evidence. Matrices misrank most risk pairs.
- Include no exploitable technical detail.
- For material incidents, mirror the SEC 8-K Item 1.05 shape: nature, scope, timing, impact.

### H. Agent context file (CLAUDE.md, AGENTS.md, skill, tool description)
Keep it short and specific:
- Hard constraints and explicit "do not" rules.
- Exact commands for build, test and lint.
- An index pointing to version-matched docs.
- Runnable examples for unfamiliar APIs.

Leave out project overviews the agent can read from the code, generic style exhortations, and anything unverified. Wrong docs hurt agents more than missing ones. Put constraints in schemas (enums, required fields) rather than prose, and list full vocabularies instead of giving only examples. Validate changes with an eval or a test task.

## Review checklist

Answer YES / NO / N/A with a quoted span or tool output as evidence.

**0. Setup**
- 0.1 Doc type, target reader, assumed expertise and reader task stated?
- 0.2 Source material read before judging?

**1. Mechanical (run tools, don't judge by eye)**
- 1.1 Every identifier, flag, endpoint, parameter and version found in source? List any misses.
- 1.2 Every example and command executed? Paste the result or mark it untested.
- 1.3 Every link and internal reference resolves?
- 1.4 For rewrites: is anything omitted (precondition, caveat, number) or added that isn't in the source?

**2. What is this / is it for me?**
- 2.1 First 1–3 sentences give what, for whom, what the reader can do, and status?
- 2.2 Scope stated: versions, environments, supported or deprecated?
- 2.3 Explanation/argument docs: conclusion first *and* restated at the end?

**3. Can I find it?**
- 3.1 Headings name tasks or objects and front-load the distinguishing words? No generic labels?
- 3.2 Headings alone show the structure?
- 3.3 Format matches the task: steps for procedures, tables for lookup?
- 3.4 Page stands alone for a reader arriving from search, with prerequisites linked?

**4. Can I understand it?**
- 4.1 Every acronym and coined term defined or linked at first use?
- 4.2 Verbs instead of nominalizations? Quote the worst instance.
- 4.3 Concepts introduced before or beside the examples that need them?
- 4.4 Emphasis sparse (≤1 bold thesis per section, no one-line-bullet runs)?
- 4.5 Novice path and expert reference separated (levels)?
- 4.6 Reasoning in prose, enumerations in lists (no "because" inside bullets)?

**5. Can I act on it correctly?**
- 5.1 Numbered one-action steps, with the goal stated before them?
- 5.2 Prerequisites and expected result of each critical step stated?
- 5.3 Recovery info at the step where the failure occurs?
- 5.4 Leading example complete, copy-pasteable, task-like and secure by default?
- 5.5 Example explained (why it works and why it's the safe choice)?
- 5.6 Asks ranked, with the top one or two marked?

**6. Can I trust it?**
- 6.1 Owner and last-verified date or version present?
- 6.2 Claims that matter cite a source, ticket or test?
- 6.3 Facts declarative, judgments first person with confidence? No unsupported reassurance?
- 6.4 Numbers instead of adjectives? Probabilities as words plus numbers?

**7. Security add-ons (by doc type)**
- 7.1 Paved road: secure option first and easiest? Insecure shortcuts never shown as copyable defaults? Guardrail coverage and gaps stated?
- 7.2 Finding: class, title, location, why, preconditions, repro, true-positive check, fix/patch, case-vs-machine, link?
- 7.3 CVSS labeled by version and metric group, with assumptions stated?
- 7.4 VEX status with justification or action statement?
- 7.5 Runbook: exact commands, if/then decisions, escalation, rollback, runnable as-is?
- 7.6 Exec: ask first, business impact, words plus numbers, what would change the call, no heat map as primary evidence, no exploitable detail?

**8. LLM-draft hygiene**
- 8.1 No inflated significance, promotional tone or vague trailing "-ing" clauses? Quote any.
- 8.2 No restated question, stacked hedges or padding?
- 8.3 No slogans (a sentence that could hang on an office wall)?

**Design feedback (design feedback mode only)**
- F.1 Can the author tell from the first 3 lines whether they can proceed, and what the top fix is?
- F.2 Every major decision checked for issue, options and rationale? At least one real alternative raised where none existed?
- F.3 At least 2 failure or abuse scenarios supplied that the author didn't cover?
- F.4 Identification, authorization, validation, exposure, audit and privilege checked at every trust boundary before crypto?
- F.5 Every threat names asset, boundary, attacker capability and assumption, and threats are ranked?
- F.6 Agentic components checked for injection paths, tool privilege, identity, human approval and supply chain?
- F.7 Every comment has a label, location and concrete suggestion? Every Blocking names a failure scenario?
- F.8 No comment about the person, no vague negative, no "why" longer than one line, ≤ about 10 comments?

**9. Gate**
- 9.1 Reader-test kit produced (5–8 scenario questions with answers and locations)?
- 9.2 Doc marked "reviewed, not reader-tested" until ≥2 rounds pass 90/90?

## Do not
- Invent identifiers, defaults, limits or behavior. Write a TODO instead.
- Show an insecure pattern as the main example, even "for simplicity." If a shortcut is insecure, say so on that line.
- Use Dalio section names as headings in technical docs. Rename them to their content.
- Chase readability scores, ban the passive voice wholesale, or cut content a reader needs just to be shorter.
- Grade your own draft in the same pass. Declare a doc "done" on review alone.
- In design feedback, pad with low-confidence comments, list STRIDE categories without assumptions, or critique the author instead of the design.
- Attribute rules to Dalio that aren't documented.
