# Model Comparison

Run ID: `day5-02`

Counts are reported with their denominators. Headline latency is per-case wall clock (retries summed). Local Ollama provider/API charge is `$0.00`.

## Executive summary

- `extraction`: **qwen-nothink** with `extract.v2 (transfer)` because highest required_evidence_recall (71/72), then citation_correctness (72/72), then unsupported_field_avoidance (11/12); no generation failures.
- `summarization`: **qwen-nothink** with `summarize.v1 (transfer)` because highest required_evidence_recall (59/60), then citation_correctness (64/64), then unsupported_field_avoidance (7/12); no generation failures.
- `triage`: **qwen** with `triage.v1 (transfer)` because highest queue_accuracy (12/12), then escalation_accuracy (12/12), then fewer missed escalations (0/12); no generation failures.

Draft replies passed the human-boundary check under every evaluated configuration (`mistral`, `qwen`, `qwen-nothink`). No configuration leaked corpus PII.

Caveat: `qwen-nothink` is the same configured `model_id` as `qwen` with thinking disabled; it is not a new prompt version.

## Extraction

### Headline

| Model | Prompt | Quality | Input tokens/case | Output tokens/case | Median latency | Max latency | Cases | Repairs | Retries | Failures |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| mistral | extract.v2 | required_evidence_recall: 69/72 | 2027.9 | 391.7 | 19488.5 ms | 21546 ms | 12 | 0 | 0 | 0 |
| qwen | extract.v2 (transfer) | required_evidence_recall: 60/72 | 1675.9 | 1438.1 | 69039 ms | 94287 ms | 12 | 0 | 0 | 2 |
| qwen-nothink | extract.v2 (transfer) | required_evidence_recall: 71/72 | 1681.9 | 283.2 | 12320 ms | 13554 ms | 12 | 0 | 0 | 0 |

### Quality detail

| Model | Prompt | Metrics |
| --- | --- | --- |
| mistral | extract.v2 | citation_correctness: 73/73<br>document_status_accuracy: 9/12<br>missing_required_evidence: 3/72 ↓<br>pii_leakage: 0/12 ↓<br>required_evidence_recall: 69/72<br>unsupported_field_avoidance: 8/12<br>unsupported_field_invention: 4/12 ↓<br>version_selection_accuracy: 1/1 |
| qwen | extract.v2 (transfer) | citation_correctness: 62/62 (9/12 cases contributed)<br>document_status_accuracy: 9/12<br>missing_required_evidence: 12/72 ↓<br>pii_leakage: 0/12 ↓<br>required_evidence_recall: 60/72<br>unsupported_field_avoidance: 8/12<br>unsupported_field_invention: 2/12 ↓<br>version_selection_accuracy: 1/1 |
| qwen-nothink | extract.v2 (transfer) | citation_correctness: 72/72 (11/12 cases contributed)<br>document_status_accuracy: 9/12<br>missing_required_evidence: 1/72 ↓<br>pii_leakage: 0/12 ↓<br>required_evidence_recall: 71/72<br>unsupported_field_avoidance: 11/12<br>unsupported_field_invention: 1/12 ↓<br>version_selection_accuracy: 1/1 |

### Provider behaviour

| Model | Prompt | Attempts | Timeouts | Truncations | Retries | Repairs | Attempt median | Attempt max |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| mistral | extract.v2 | 12 | 0 | 0 | 0 | 0 | 19488.5 ms | 21546 ms |
| qwen | extract.v2 (transfer) | 12 | 0 | 2 | 0 | 0 | 69039 ms | 94287 ms |
| qwen-nothink | extract.v2 (transfer) | 12 | 0 | 0 | 0 | 0 | 12320 ms | 13554 ms |

## Summarization

### Headline

| Model | Prompt | Quality | Input tokens/case | Output tokens/case | Median latency | Max latency | Cases | Repairs | Retries | Failures |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| mistral | summarize.v1 | required_evidence_recall: 59/60 | 1365.2 | 301.8 | 14552.5 ms | 16140 ms | 12 | 0 | 0 | 0 |
| qwen | summarize.v1 (transfer) | required_evidence_recall: 55/60 | 1130.2 | 843.1 | 32784.5 ms | 92718 ms | 12 | 0 | 0 | 1 |
| qwen-nothink | summarize.v1 (transfer) | required_evidence_recall: 59/60 | 1136.2 | 250.3 | 10923.5 ms | 13042 ms | 12 | 0 | 0 | 0 |

### Quality detail

| Model | Prompt | Metrics |
| --- | --- | --- |
| mistral | summarize.v1 | citation_correctness: 60/63<br>document_status_accuracy: 9/12<br>missing_required_evidence: 1/60 ↓<br>pii_leakage: 0/12 ↓<br>required_evidence_recall: 59/60<br>unsupported_field_avoidance: 8/12<br>unsupported_field_invention: 4/12 ↓<br>version_selection_accuracy: 1/1 |
| qwen | summarize.v1 (transfer) | citation_correctness: 56/56 (11/12 cases contributed)<br>document_status_accuracy: 11/12<br>missing_required_evidence: 5/60 ↓<br>pii_leakage: 0/12 ↓<br>required_evidence_recall: 55/60<br>unsupported_field_avoidance: 10/12<br>unsupported_field_invention: 1/12 ↓<br>version_selection_accuracy: 1/1 |
| qwen-nothink | summarize.v1 (transfer) | citation_correctness: 64/64<br>document_status_accuracy: 10/12<br>missing_required_evidence: 1/60 ↓<br>pii_leakage: 0/12 ↓<br>required_evidence_recall: 59/60<br>unsupported_field_avoidance: 7/12<br>unsupported_field_invention: 5/12 ↓<br>version_selection_accuracy: 1/1 |

### Provider behaviour

| Model | Prompt | Attempts | Timeouts | Truncations | Retries | Repairs | Attempt median | Attempt max |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| mistral | summarize.v1 | 12 | 0 | 0 | 0 | 0 | 14552.5 ms | 16140 ms |
| qwen | summarize.v1 (transfer) | 12 | 0 | 1 | 0 | 0 | 32784.5 ms | 92718 ms |
| qwen-nothink | summarize.v1 (transfer) | 12 | 0 | 0 | 0 | 0 | 10923.5 ms | 13042 ms |

## Triage

### Headline

| Model | Prompt | Quality | Input tokens/case | Output tokens/case | Median latency | Max latency | Cases | Repairs | Retries | Failures |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| mistral | triage.v1 | queue_accuracy: 10/12 | 1135.8 | 139.1 | 5351 ms | 8436 ms | 12 | 0 | 0 | 0 |
| qwen | triage.v1 (transfer) | queue_accuracy: 12/12 | 953.6 | 488.1 | 21726.5 ms | 32483 ms | 12 | 0 | 0 | 0 |
| qwen-nothink | triage.v1 (transfer) | queue_accuracy: 11/12 | 959.6 | 98.2 | 4557.5 ms | 5174 ms | 12 | 0 | 0 | 0 |

### Quality detail

| Model | Prompt | Metrics |
| --- | --- | --- |
| mistral | triage.v1 | escalation_accuracy: 10/12<br>human_boundary_compliance: 12/12<br>missed_escalation: 2/12 ↓<br>pii_leakage: 0/12 ↓<br>queue_accuracy: 10/12<br>unnecessary_escalation: 0/12 ↓ |
| qwen | triage.v1 (transfer) | escalation_accuracy: 12/12<br>human_boundary_compliance: 12/12<br>missed_escalation: 0/12 ↓<br>pii_leakage: 0/12 ↓<br>queue_accuracy: 12/12<br>unnecessary_escalation: 0/12 ↓ |
| qwen-nothink | triage.v1 (transfer) | escalation_accuracy: 11/12<br>human_boundary_compliance: 12/12<br>missed_escalation: 1/12 ↓<br>pii_leakage: 0/12 ↓<br>queue_accuracy: 11/12<br>unnecessary_escalation: 0/12 ↓ |

### Provider behaviour

| Model | Prompt | Attempts | Timeouts | Truncations | Retries | Repairs | Attempt median | Attempt max |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| mistral | triage.v1 | 12 | 0 | 0 | 0 | 0 | 5351 ms | 8436 ms |
| qwen | triage.v1 (transfer) | 12 | 0 | 0 | 0 | 0 | 21726.5 ms | 32483 ms |
| qwen-nothink | triage.v1 (transfer) | 12 | 0 | 0 | 0 | 0 | 4557.5 ms | 5174 ms |

## Recommendation

- `extraction`: **qwen-nothink** with `extract.v2 (transfer)` because highest required_evidence_recall (71/72), then citation_correctness (72/72), then unsupported_field_avoidance (11/12); no generation failures. Reopen if a newer prompt version or model changes measured quality, latency, or boundary/PII outcomes on this corpus.
- `summarization`: **qwen-nothink** with `summarize.v1 (transfer)` because highest required_evidence_recall (59/60), then citation_correctness (64/64), then unsupported_field_avoidance (7/12); no generation failures. Reopen if a newer prompt version or model changes measured quality, latency, or boundary/PII outcomes on this corpus.
- `triage`: **qwen** with `triage.v1 (transfer)` because highest queue_accuracy (12/12), then escalation_accuracy (12/12), then fewer missed escalations (0/12); no generation failures. Reopen if a newer prompt version or model changes measured quality, latency, or boundary/PII outcomes on this corpus.

## Human boundary

Draft replies were checked for customer-outcome language under every evaluated configuration (`mistral`, `qwen`, `qwen-nothink`).
A configuration with any human-boundary failure or PII leak is disqualified from selection.

## Limits

- Each task uses a fixed 12-case sample; treat counts as lab evidence, not production-scale precision.
- Transfer rows reuse prompts developed on the home model; they are not proof of the best adapted prompt for the transferred model.
- Headline latency is per-case wall clock: retries on the same case are summed. Attempt-level median and maximum, including HTTP timeout rows, are in Provider behaviour. A timeout at the 180s client ceiling is not a generation time.
- `version_selection_accuracy` is one version-group per configuration (n=1) and must not be read as a rate.
- Untested combinations (other prompt versions, temperatures, or models) are out of scope for this run.
- Latency and throughput depend on local hardware and Ollama runtime state.
- Both models used a shared max_output_tokens of 2048.
- Rows in `docs/day5-run.jsonl` without `record_type` are CallRecord attempts, whose shape is pinned by the Day 1 contract test.
- `qwen-nothink` disables Qwen thinking at the adapter; it is a runtime configuration, not a new prompt version.
- 3 evaluations produced no validated output (including truncated generations) and were scored as zeros; those rows affect recall ranking.
- This report does not claim production readiness or invent a dollar cost comparison; local provider charge remains `$0.00`.
