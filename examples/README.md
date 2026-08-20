# Examples

A small, **sanitized** set of real cases — enough to show a failure going in and a grounded,
cited answer coming out, without exposing anything sensitive.

Each example is expected to contain:

- **`LABEL.txt`** — the case's answer key: protocol, expected cause, expected TS + clause. (Clean —
  no identifiers.)
- **A trimmed Trust‑Sandwich report** — the Retrieval Health Card, the analysis, and the Citation
  Audit (VERIFIED / INVENTED). The full 3GPP clause *text* is omitted (copyright); only the citation
  labels (e.g. `TS 24.501 §5.5.1`) are kept.

Everything here is scrubbed of subscriber identifiers, PLMN/DNN values, device names, and lab
infrastructure details (IPs, hostnames). Raw captures and the spec index are **not** included.

## Included case

- [`TC-5GMM-011-plmn-not-allowed/`](TC-5GMM-011-plmn-not-allowed/) — a real reproduced *PLMN not
  allowed* failure (5GMM cause #11). Contains the [`LABEL.txt`](TC-5GMM-011-plmn-not-allowed/LABEL.txt)
  answer key and a trimmed [`REPORT.md`](TC-5GMM-011-plmn-not-allowed/REPORT.md) — Health Card 98% →
  PASS, correct spec, zero invented citations.
