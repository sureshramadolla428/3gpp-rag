# Design notes — why 3GPP-RAG is built the way it is

*The reasoning behind the pipeline, for anyone reading the code or the results.*

## The two problems that shaped everything

When a 5G registration fails, the question is always: *what does the spec say should have happened,
and where did this run diverge?* Two things make the obvious answer — "ask an LLM" — a trap:

1. **Hallucination.** A model that confidently states "cause #11 means X, see TS 24.501 §9.9.9" —
   when that section doesn't say it, or doesn't exist — is worse than no tool. It sends you the wrong
   way and erodes trust in the whole approach.
2. **Data sensitivity.** Signalling logs carry subscriber, device, and operator identifiers. They
   can't be pasted into a hosted service.

So the design starts from two non‑negotiables: **everything runs offline**, and **the specification —
not the model's memory — is the source of truth**, with every citation verified.

## The core move: retrieve, then *audit*

Retrieval‑augmented generation is the starting point — retrieve the relevant 3GPP clauses and ask
the model to explain the failure using only those. But retrieval alone isn't enough: a model can
still paraphrase a clause into something it doesn't say, or cite a section that was never retrieved.

3GPP-RAG adds the step most demos skip — a **citation audit**. After the model answers, every
`TS xx.xxx §y.y.y` it cited is checked against the exact retrieved text. Anything unbacked is flagged
**invented**, and the system re‑searches and regenerates. The output isn't "trust me"; it's "here's
the answer, and here's the proof, audited."

## Determinism where it matters

The failure's protocol, message, and cause come from **parsing the trace**, not from the model. The
model's job is narrow: explain a *known* failure using *provided* clauses. That boundary is what
keeps it honest — and it's why the citation audit can be strict.

## What turned out to be hard

- **Reproducing failures.** Many cause codes are easy to name and hard to actually trigger on an
  open‑source stack. A reproduction *gate* only counts a case if its signature shows up in the
  decoded capture — no gate, no green check.
- **Spec metadata.** A searchable index is only as useful as its section labels. Making each chunk
  carry its real clause number — so a citation reads `§5.5.1`, not just "page 1093" — took a
  dedicated pass that carries each heading forward to its continuation chunks.
- **Defining "correct."** For a rejection, is the right citation the cause‑code table or the
  procedure clause? Often both are valid, and scoring has to respect that.

## Honest state

This is a prototype. Retrieval grounding and the citation‑audit guardrail are working on real
reproduced failures; the quality of the *generated explanations* and the breadth of coverage are
still being measured. See [`../ROADMAP.md`](../ROADMAP.md). The results shown are a real, modest
signal — not a polished headline.
