# Dalio-style writing skills

Claude skills for writing work documents that are clear, written to their reader, and human rather than corporate.

`writing-partner` is the general skill. It drafts, critiques and edits any business writing (emails, Slack posts, memos, announcements, bad-news notes, posts, bios), checks drafts with a reviewer that never saw the working session, asks the author for missing facts instead of inventing them, and can learn the author's voice from samples. For the document types below, it hands structure to the Dalio skills and handles wording, voice, review and editing.

The Dalio skills produce documents in the style of Ray Dalio: layered (nutshell first), mechanism before cases, explicit confidence and odds, named owners, and data presented as sourced gauges. They add guardrails against the weaknesses critics point out in his writing: bullet-heavy "PowerPoint" structure, platitudes, cherry-picked templates, unsourced charts, and person-level blame.

| Skill | Use it for |
|---|---|
| [`writing-partner`](writing-partner/SKILL.md) | Drafting, critiquing and editing any business writing; building a voice profile |
| [`dalio-decision-memo`](dalio-decision-memo/SKILL.md) | Decision memos and decision-meeting pre-reads |
| [`dalio-strategy-analysis`](dalio-strategy-analysis/SKILL.md) | Strategy and planning docs; recurring status and risk reports |
| [`dalio-diagnosis-principles`](dalio-diagnosis-principles/SKILL.md) | Postmortems and diagnoses (blameless by default); team operating principles |

## Install

Copy a skill folder into your skills directory (for Claude Code, `~/.claude/skills/`), or upload the folder as a skill in Claude. `writing-partner` keeps its rubric, pattern catalog, reviewer prompts and evidence in `references/`, so copy the whole folder.

## Basis

The Dalio skills come from research into Dalio's *Principles*, *Big Debt Crises*, *The Changing World Order* and *How Countries Go Broke*, his essays, the documented Bridgewater protocols, and third-party critiques. Techniques with no source (for example "1-2-3 go") are deliberately not attributed to him.

`writing-partner` draws on field experiments with real readers (Rogers and Lasky-Fink's *Writing for Busy Readers*, studies of formality, jargon, concreteness and hedging), the writing canon (Williams, Pinker, Orwell, Roman and Raphaelson, Garner, institutional style guides), research on how LLM prose differs from human prose, and research on LLM critique and editing. Its `references/evidence.md` lists sources and marks what is tested, what is consensus, and what is untested reasoning.
