# cite-check

**One claim, one best citation, PMID-anchored and tier-graded**

Splits prose into atomic claims, searches PubMed/Europe PMC (and preprints), retrieves and reads each abstract, and returns the single strongest primary reference per claim with a verifiable link and an evidence tier (A–D, R). Claims with no adequate primary source are reported as gaps rather than papered over.

A Claude Code plugin — it installs a skill that triggers automatically when you ask for citations, so you rarely need to name it.

## Install

```
/plugin marketplace add snersesian/cite-check
/plugin install cite-check@cite-check
```

## Recommended setup: connect a PubMed MCP

cite-check works out of the box using web search, but it is **noticeably more reliable with a PubMed MCP connected** — it then pulls structured records straight from NCBI (PMID, DOI, `PublicationType`, abstract), which is what lets a citation reach *High* confidence and makes the retraction check dependable. Without one, it falls back to web retrieval and says so at the top of its output.

Any PubMed / E-utilities MCP works. Options, easiest first:

- **Anthropic's PubMed connector** — the lowest-friction choice, no local runtime to manage. See [Using the PubMed connector in Claude](https://claude.com/resources/tutorials/using-the-pubmed-connector-in-claude).
- **Community MCP servers** (need a local Python/Node runtime): e.g. [JackKuo666/PubMed-MCP-Server](https://github.com/JackKuo666/PubMed-MCP-Server) or the multi-source [openags/paper-search-mcp](https://github.com/openags/paper-search-mcp) (PubMed + arXiv + bioRxiv). Follow that project's README for the exact `mcpServers` entry.

> An NCBI API key (free) raises rate limits; add it to the server's env if it supports one.

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
