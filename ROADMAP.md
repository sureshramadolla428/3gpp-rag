# Roadmap & status

3GPP-RAG is an honest work in progress. This page tracks what is proven, what is being measured,
and what's next - so nothing here reads as more finished than it is.

## Done

- [x] Opensource 5G test lab (Ubuntu - UERANSIM - Open5GS - OpenAirInterface) + a faultinjection
      testcase suite (one case per 3GPP cause code) with percase answer keys, to reproduce and score real failures.
- [x] Offline redaction pipeline (removes subscriber / device / operator identifiers).
- [x] Parse + consolidate signalling into perUE timelines and a portable percapture capsule.
- [x] Local 3GPP spec index (SQLite FTS5 / BM25) with clauselevel section metadata.
- [x] Grounded generation via a local model, fenced to retrieved clauses only.
- [x] Citation audit (VERIFIED vs INVENTED) and selfcorrection loop.
- [x] "Trust Sandwich" report (health card + analysis + citation audit).
- [x] Reproducedfailure eval harness that scores answers against a percase answer key
      (protocol + cause + TS/clause).

## Proven so far

On a set of failures reproduced in opensource labs (OpenAirInterface + UERANSIM / Open5GS):

- Retrieval grounded to the **correct specification in every tested case**.
- **Zero hallucinated citations** across the tested set.

*Small, early evaluation set - treated as a signal, not a headline number.*

## In progress

- [ ] Full answerquality evaluation (measuring the generated explanations, not just retrieval).
- [ ] Execute the complete failure testsuite (many cases still pending reproduction).
- [ ] Clauseprecision: prefer the definitional cause clause *and* the procedure clause where both apply.

## Next

- [ ] Broaden beyond L3 NAS - NGAP / F1AP / RRC, and L1 / L2 measurement context.
- [ ] Nonterrestrial network (NTN) failure modes.
- [ ] Batch mode: score a whole capture, not one failure at a time.
- [ ] Public release of the processing scripts (after the testsuite run is complete).

## Nongoals

- Shipping 3GPP specification text (copyrighted - build the index locally).
- Any cloud dependency. If a step needs the network, it doesn't belong here.
