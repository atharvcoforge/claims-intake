# Model Decision Record

Run ID: `day5-02`

Selection follows the committed Day 5 rule: triage prefers queue_accuracy, then escalation_accuracy, then fewer missed escalations, and disqualifies human-boundary or PII failures; summarization/extraction prefer required_evidence_recall, then citation_correctness, then unsupported_field_avoidance; ties break on median latency, then output tokens per case.

## Evaluated models

- mistral
- qwen
- qwen-nothink

## Evidence rows

- `extraction` / mistral / `extract.v2`
- `extraction` / qwen / `extract.v2 (transfer)`
- `extraction` / qwen-nothink / `extract.v2 (transfer)`
- `summarization` / mistral / `summarize.v1`
- `summarization` / qwen / `summarize.v1 (transfer)`
- `summarization` / qwen-nothink / `summarize.v1 (transfer)`
- `triage` / mistral / `triage.v1`
- `triage` / qwen / `triage.v1 (transfer)`
- `triage` / qwen-nothink / `triage.v1 (transfer)`

## Task decisions

### extraction

- selected model: qwen-nothink
- prompt version: `extract.v2 (transfer)`
- measured reason: highest required_evidence_recall (71/72), then citation_correctness (72/72), then unsupported_field_avoidance (11/12); no generation failures
- rejected alternative(s): mistral/extract.v2 (69/72); qwen/extract.v2 (transfer) (60/72, 2 failed (12 of 12 missing required fields from cap-truncated cases))
- reopen when: a newer prompt version or model changes measured quality, latency, or boundary/PII outcomes on this 12-case corpus

### summarization

- selected model: qwen-nothink
- prompt version: `summarize.v1 (transfer)`
- measured reason: highest required_evidence_recall (59/60), then citation_correctness (64/64), then unsupported_field_avoidance (7/12); no generation failures
- rejected alternative(s): mistral/summarize.v1 (59/60); qwen/summarize.v1 (transfer) (55/60, 1 failed (5 of 5 missing required fields from cap-truncated cases))
- reopen when: a newer prompt version or model changes measured quality, latency, or boundary/PII outcomes on this 12-case corpus

### triage

- selected model: qwen
- prompt version: `triage.v1 (transfer)`
- measured reason: highest queue_accuracy (12/12), then escalation_accuracy (12/12), then fewer missed escalations (0/12); no generation failures
- rejected alternative(s): mistral/triage.v1 (10/12); qwen-nothink/triage.v1 (transfer) (11/12)
- reopen when: a newer prompt version or model changes measured quality, latency, or boundary/PII outcomes on this 12-case corpus
