Research Ledger — Commit-Reveal Protocol
Purpose: demonstrate research process integrity, not market performance.
Protocol
	1.	At decision time, a full call record (ticker, scores, thesis, entry logic) is written privately.
	2.	Its SHA-256 hash is committed publicly immediately — Git timestamps it.
	3.	After 60–90 days (or position closure), the plaintext is published and must match the hash.
	4.	Every committed call is eventually revealed — wins, losses, and aborted setups. No survivorship editing.
Redacted even at reveal
Scoring weights, theme codes, position sizes.
Disclaimer
Educational research documentation. Nothing here is investment advice, a performance claim, or a solicitation.

4. llm-reliability-patterns/README.md — fully open source

Generic, IP-safe engineering patterns for forcing honesty in LLM data pipelines: receipt-echo verification, provenance tagging (DIRECT / COMPUTED / OVERRIDE / MISSING), copy-forward baseline locks, regression diff guards, schema drift detection, atomic step execution, self-check gates, halt-over-guess protocols. Each pattern documented with a generic worked example, failure mode it prevents, and copy-paste template.

This repo earns the “AI prompt engineer” credibility on its own — it’s real engineering, none of it touches scoring logic, and it’s the one that collects stars.