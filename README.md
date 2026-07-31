# cite-check

**One claim, one best citation, PMID-anchored and tier-graded**

Splits prose into atomic claims, searches PubMed/Europe PMC (and preprints), retrieves and reads each abstract, and returns the single strongest primary reference per claim with a verifiable link and an evidence tier (A–D, R). Claims with no adequate primary source are reported as gaps rather than papered over.

A Claude Code plugin — it installs a skill that triggers automatically when you ask for citations, so you rarely need to name it.

## Install

```
/plugin marketplace add snersesian/cite-check
/plugin install cite-check@cite-check
```

> Tip: retrieval is noticeably more reliable if you also connect a **PubMed MCP** server — cite-check will use it for search and record retrieval and fall back to web search when it isn't present.

## Ask for it like this

- "Find me the best citation for each sentence in this paragraph"
- "Is this claim actually supported? PMIDs please"
- "Grade the evidence behind these five statements"
- "Has anyone shown X?" / "What's known about Y?" (landscape / gap mode)

## What it does and doesn't do

- **Does:** biomedical literature — claim-backing for manuscripts, grants, and reviews, plus novelty/gap landscape questions. Grades evidence by study design and flags claims whose wording outruns the evidence.
- **Doesn't:** search your own data, protocols, or lab notebook; read full-text (it grades on abstracts and says so when an abstract is ambiguous); screen hundreds of papers at once. It is a citation/verification layer, not a general search engine.

## Notes

Built around never citing a paper it hasn't retrieved. A fabricated reference is worse than a missing one.

---

Made for the Cook Lab · MIT
