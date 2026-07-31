# Evidence tiers

Grade the study design, not the journal. A Tier C result in a high-impact journal is
still Tier C, and reviewers grade it that way.

## Provenance & confidence (report alongside the tier)

The tier says *how strong the design is*. Two more fields say *how the conclusion was
mapped* and *how sure you are* — report both for every citation.

**Design & scale** — the one line that lets a reader judge without opening the paper:

| Design | Typical n unit |
|---|---|
| Human clinical cohort (survival / prospective) | patients |
| Human primary tissue / ex vivo | samples or donors |
| In vivo animal (name the model + perturbation) | mice per arm |
| In vitro (cell line / organoid / primary cells) | cell lines / replicates |
| Computational / database (e.g. TCGA, GTEx) | tumours / records |
| Review / meta-analysis | studies pooled |

Always state the **n** with its unit, pulled from the abstract/methods — or `n not stated`.
Name the species/model and flag species drift (mouse mechanism cited for a human claim).

**Confidence** — orthogonal to tier; grades this paper × this claim × record certainty:

- **High** — identifier resolved, metadata confirmed, abstract states the claim directly.
- **Moderate** — supported with a caveat (species drift, indirect readout, claim slightly
  broader than the abstract, or metadata from a search snippet not a fully retrieved record).
- **Low** — tangential support, unresolved/uncertain identifier, or ambiguous abstract →
  usually belongs in the "weaker than implied" or "not found" list, not the citation body.

A Tier-A cohort can be Moderate confidence for a claim it only grazes; a Tier-C cell-line
paper can be High confidence for a narrow in-vitro claim it states outright.

## Tier A — clinical or independently replicated

- Randomised controlled trial, or prospective clinical cohort with a prespecified endpoint
- Large annotated tissue cohort or tissue microarray with survival data (order 100s–1000s)
- Multi-cohort study with an independent validation set
- Meta-analysis of primary studies
- A finding independently replicated by a separate group in separate material

For biomarker and prognostic claims this is the only tier that supports language like
"is associated with outcome" without hedging.

## Tier B — in vivo or primary human material with function

- Genetic model (knockout, knock-in, conditional) with a functional phenotype
- Patient-derived material — organoids, primary cells, ex vivo tissue — with a
  functional readout, not just expression
- Xenograft or syngeneic model with intervention and control arms
- Single-cell or spatial data from primary patient tissue with orthogonal validation

Supports mechanistic claims. Flag the species if the claim is about humans.

## Tier C — single system, unreplicated

- One immortalised cell line
- One model system with no orthogonal confirmation
- A single cohort with no validation set
- Overexpression or knockdown in vitro without in vivo follow-up

Supports "has been reported to" and "in vitro evidence suggests". Does not support
"drives", "requires", or "is necessary for".

## Tier D — computational or correlative only

- TCGA, GTEx, or other public database mining without wet-lab validation
- Co-expression, correlation, or ligand–receptor inference
- Predicted interactions from a curated database
- Spatial co-localisation without perturbation

Supports "is correlated with" and "co-localises with". Never supports a causal verb.
Ligand–receptor inference tools in particular are hypothesis-generating by their own
authors' framing, and reviewers know this.

## Tier R — review, commentary, perspective

Fine for framing a field, defining a term, or pointing at a body of work. Not a
citation for a specific factual claim. If a review is the only thing supporting a
sentence, that sentence is undersupported — follow the review's own references down
to the primary and cite that instead.

## Grading edge cases

**Preprints.** Grade on design exactly as a published paper and mark `(preprint)`.
Because it has not been peer-reviewed, cap **confidence** at Moderate regardless of tier,
and note that some journals restrict preprint citation. Preprints are essential for recall
on novelty and gap questions: a preprint that already reports X defeats a novelty claim
just as a paper would, and leaving preprints out is how a "no one has done this" statement
quietly goes stale. Always check for a published version (Europe PMC links
preprint↔published) and prefer it when it exists.

**Retracted or corrected papers.** Check retraction status before citing. This is not
optional and it is fast.

**Conference abstracts.** Tier C at best, and usually not citable. Say so.

**Very old primaries.** A 1980s paper establishing a canonical mechanism is often still
the correct citation. Age is not a demerit if nothing has superseded it — but confirm
nothing has.

## Mapping tiers to permissible verbs

| Tier | Safe verbs |
|---|---|
| A | is associated with, predicts, is prognostic for |
| B | drives, is required for, promotes (with species named) |
| C | has been reported to, can, in vitro evidence indicates |
| D | correlates with, co-occurs with, is predicted to |
| R | has been reviewed, is generally understood as |
