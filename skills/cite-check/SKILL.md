---
name: cite-check
description: Find the single strongest primary citation for each factual claim in a manuscript, grant, or review, PMID-anchored and graded by evidence tier so the author can verify every reference manually. Use this whenever the user asks for citations, references, "the best paper for this sentence", literature support, a PMID, or asks whether a claim is supported — and whenever they paste sentences from a draft and want them backed up. Also runs a landscape/discovery mode for open questions — "has anyone shown X?", "what's known about Y?", "is this novel?" — where the job is to map what exists and where the gap is, not to back a sentence. Searches preprints (bioRxiv/medRxiv, Europe PMC PPR filter) alongside peer-reviewed literature so novelty and gap claims are not judged on published work alone. Prefer this over answering from memory, because a plausible-looking citation that does not exist is worse than no citation.
allowed-tools: Bash, Read, WebSearch, WebFetch
---

# Cite Check

One claim, one best citation, one verifiable link, one honest grade.

The failure mode this skill exists to prevent is the confident fabricated reference:
a real-sounding title attached to a real-sounding journal that does not exist, or a
real paper that does not actually say the thing it is cited for. Everything below is
built around making that impossible rather than unlikely.

## Non-negotiables

**Never cite a paper without retrieving it.** Search, then fetch the record, then read
the abstract. If a PMID or DOI cannot be resolved, the citation does not go in the
output — it goes in the "not found" list.

**Never assert what a paper shows beyond what the retrieved abstract supports.** If
the abstract is ambiguous about the specific claim, say so and grade it down. The
author is going to check.

**One citation per claim.** The request is for the strongest reference, not a menu.
Offering five options moves the work back onto the author, which is the thing they
were trying to avoid. Name a second only when the claim genuinely rests on two
independent legs (for example, a discovery paper and its independent replication).

## Two modes

Infer which mode the request wants; when genuinely ambiguous, ask before searching.

**Claim-backing (default).** The input is prose with assertions — a paragraph, an
abstract, a list of sentences — and the job is one best citation per claim. This is the
numbered Workflow below.

**Landscape / discovery.** The input is an open question — "has anyone shown X?", "what
data exist for Y?", "is this novel?" — with no sentence to back. Do **not** invent claims
to cite; fabricating a proposition just to attach a reference inverts the tool's purpose
and manufactures false precision. Instead run **Landscape mode** (bottom of this file):
recall first, then the same retrieve-and-grade gate, and report a graded map of what
exists and where the gap is. The rigor is identical; only the shape of the input and the
output changes.

## Workflow

### 1. Split the input into atomic claims

Take the pasted prose and break it into individually citable propositions. A sentence
containing three assertions needs three citations, and silently citing only the first
is a common and invisible error. Number them and show the split before searching, so
the author can correct the parse.

### 2. Search

Use the PubMed MCP if connected; otherwise use web search against PubMed and Europe PMC.
Search preprint servers in the same pass (see the preprint bullet below) — peer review
lags, and for novelty/gap work the relevant paper is often not yet published.
Query construction matters more than query count:

- Pair a **mechanism term** with a **cell type or model** and a **disease context**.
  Broad single-concept queries return reviews; specific mechanistic queries return primaries.
- Query naming conventions the field actually uses. Nomenclature invented for a
  particular paper or lab returns nothing — search the underlying receptor, ligand,
  or function instead.
- Run one specific and one broad query per claim. The broad query reliably surfaces
  papers the specific one misses, and the pair takes little longer than either alone.
- Watch for synonym expansion inflating hit counts. Acronyms that collide with common
  assay names are the usual culprit; raw hit numbers cannot be taken at face value.
- **Include preprints.** Query bioRxiv/medRxiv (their API, or a preprint MCP if
  connected) and add Europe PMC's `SRC:PPR` preprint filter to at least the broad query.
  This is the tool's main coverage blind spot: a preprint that already reports X defeats a
  novelty claim exactly as a published paper would, and omitting preprints is how a "no one
  has done this" statement quietly goes stale. Grade a preprint on its design like anything
  else, mark it `(preprint)`, and cap its confidence at Moderate (unrefereed) — see
  `references/evidence-tiers.md`. Check Europe PMC for a published version and prefer it.

### 2b. Optional second broad pass (recall)

After the targeted queries, optionally run one wider, agentic sweep whose only job is to
surface what the targeted queries missed: alternate nomenclature and synonyms, related-
article links, forward/backward citation chasing from the closest hits, and a preprint-only
pass. **Everything it finds re-enters at step 3 — retrieve-and-grade — and never shortcuts
to the output.** Because the grade gate is unchanged, this buys recall without spending
precision. Reach for it whenever the claim is a novelty/gap statement, the first pass came
back thin, or you are in Landscape mode (where it is the default, not the option).

### 3. Retrieve metadata

Batch PMIDs (up to about 20 per call) and pull titles, journals, years, DOIs, and
abstracts. Read the abstract against the specific claim, not the general topic.

While reading each abstract, extract the **provenance** the reader needs to judge the
claim without opening the paper:

- **Design** — how the conclusion was actually reached. Use one of: `Human clinical
  cohort` · `Human primary tissue / ex vivo` · `In vivo animal` · `In vitro (cell line /
  organoid / primary cells)` · `Computational / database` · `Review / meta-analysis`.
  Name the perturbation if there is one (KO, knockdown, overexpression, intervention arm).
- **Species / model** — human, mouse, specific line or PDX; flag species drift when the
  claim is human but the evidence is not.
- **Scale (n)** — the number that matters for *this* claim, with its unit: n = patients,
  tissue samples, mice/arm, cell lines, or cohorts. Pull it from the abstract/methods,
  not the framing. If the abstract does not state it, write `n not stated` — never guess.

### 4. Grade

Read `references/evidence-tiers.md` for the full rubric. The short version, strongest
first:

| Tier | Meaning |
|---|---|
| **A** | Large clinical cohort, validated tissue cohort, RCT, or independent replication |
| **B** | In vivo model or primary patient material with functional validation |
| **C** | Single cell line, single model system, or a single unreplicated cohort |
| **D** | Bioinformatic, database-derived, or correlative only |
| **R** | Review or expert commentary — acceptable for framing, not for a specific factual claim |

Grade what the paper *did*, not the journal it appeared in.

**Tier and confidence are two different axes — report both.** Tier grades the *design's
strength* (what kind of evidence it is). **Confidence** grades *how well this specific
paper supports this specific claim, and how sure you are of the record itself*:

| Confidence | When |
|---|---|
| **High** | PMID/DOI resolved and metadata confirmed, and the abstract states the claim directly. |
| **Moderate** | The claim is supported but with a caveat — species drift, indirect readout, the claim is slightly broader than the abstract, or the metadata came from a search snippet rather than a fully retrieved record. |
| **Low** | Only tangential support, an unresolved/uncertain identifier, or the abstract is ambiguous about the specific claim. Prefer moving this to the "weaker than implied" list. |

A Tier-A cohort can still be Moderate confidence for a claim it only touches in passing;
a Tier-C cell-line paper can be High confidence for a narrow in-vitro claim it states outright.

### 5. Report

Use this structure exactly:

```markdown
### Claim 1
> The verbatim sentence or clause being cited.

**Citation.** First author et al., *Journal* Year. Title.
PMID: 12345678 — https://pubmed.ncbi.nlm.nih.gov/12345678/
DOI: 10.xxxx/yyyy

**Tier.** B — in vivo mouse model with functional knockout validation.

**Design & scale.** In vivo mouse (Prox1-CreERT2 conditional KO) · n = 8 mice/arm.
*(e.g. "Human clinical cohort · n = 512 patients, TMA with survival"; "In vitro · 2 cell
lines · n not stated"; "Computational · TCGA-OV, n = 419 tumours".)*

**Confidence.** High — PMID resolved, metadata confirmed, abstract states the claim directly.

**What it actually shows.** One or two sentences, in your own words, stating what the
abstract supports. Flag explicitly if the claim is broader than the evidence.
```

Every claim therefore carries four fields the author can scan at a glance: **Tier**
(design strength), **Design & scale** (how the conclusion was reached and on what n),
**Confidence** (how well this paper backs this claim), and **What it actually shows**.

Close with two lists, both of which are as useful as the citations themselves:

- **Claims with no adequate primary source.** Say plainly that the literature does
  not support this yet. A genuine gap is a finding, and for a manuscript it is often
  the novelty argument.
- **Claims where the best available source is weaker than the prose implies.** These
  are the reviewer-scrutiny points. Suggest hedged language.

## Common traps

**Species drift.** A mechanism established in mouse cited for a human claim needs to
say so. Most mechanistic immunology reads as settled and is murine.

**Subtype / nomenclature drift.** A paper on a broader or older disease category cited
for a narrow modern subtype needs the same flag as species drift. Ovarian is the common
case: many foundational cohorts predate the high-grade serous (HGSOC) reclassification and
report "epithelial ovarian cancer" or "serous ovarian cancer" generically — citing one for
an HGSOC-specific claim is a real, easily missed gap. Confirm the subtype the paper actually
studied, and hedge or note the drift when it is broader than the claim.

**Cohort size inflation.** If the claim needs a large cohort, verify the actual n from
the abstract or methods rather than the paper's framing. Panel sizes, cohort sizes,
and "high-plex" claims routinely do not survive checking, and citing a study as
high-powered when it is not is a research-integrity problem, not a style one.

**The convenient review.** A review that asserts the claim is not a citation for the
claim. Follow it to the primary and cite that.

## Landscape / discovery mode

Use when the question is "what exists?" rather than "back this sentence." The rigor is
identical to claim-backing — nothing appears in the output that has not been retrieved and
graded — but recall comes first and the output is a map, not a verdict.

### 1. Frame the question as a search space, not a claim

Restate the open question and list the axes that bound it — mechanism, cell type, disease,
model/assay, readout, data type. Show this decomposition before searching so the author
can correct the scope. Do not turn the question into a proposition and cite it; an open
question has no truth value to back.

### 2. Cast wide (recall first)

Run the **second broad pass (2b) by default**, across PubMed, Europe PMC (including
`SRC:PPR` for preprints), and bioRxiv/medRxiv, plus one round of related-article and
citation chasing on the closest hits. For data-availability questions, also search the
repositories directly — GEO, ArrayExpress/BioStudies, EGA, dbGaP, Zenodo, and the
single-cell portals (CELLxGENE, Broad Single Cell Portal, Human Cell Atlas) — since a
dataset can exist with no matching paper, and a paper can promise data that was never
deposited. Aim to enumerate candidates and log the queries; do not pick one yet.

### 3. Cluster the hits

Sort what came back into direct answers, adjacent work (same question, different system or
readout), and analogous work worth noting. Deduplicate, and collapse preprint↔published
pairs to the published version.

### 4. Retrieve-and-grade every candidate you will mention

Exactly steps 3–4 of the main Workflow: fetch the record, read the abstract (or the
repository landing page) against the question, assign **tier · design & scale ·
confidence**. A candidate that cannot be retrieved is named in "not found," never asserted.
For a dataset, record what actually lets a reader reuse it: accession, assay, n and unit
(patients / samples / cells), tissue vs blood, and access tier (open vs controlled).

### 5. Report as a landscape

- **Closest prior art / directly relevant data** — the handful that most directly address
  the question, each graded, with accessions for datasets.
- **Adjacent evidence** — related but not direct; state precisely how it differs (wrong
  compartment, wrong disease, mouse not human, tumour not blood).
- **The gap** — say plainly what has *not* been done or deposited. For a novelty or
  grant-significance argument this is the payload: be specific about what would need to
  exist and does not.

A landscape answer of the form "the direct study does not exist; the two closest are X
(Tier B, mouse spleen) and Y (Tier D, correlative, n = 11 blood samples, GEO GSE######)"
is more useful, and more honest, than a confident single citation to something adjacent.
