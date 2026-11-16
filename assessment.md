# LogBeacon Security Log Assessment

Generated from the ISC-2083 Lab 5 Jupyter run on November 15, 2026. The input was a private Windows Security log export. The public report omits raw log entries, usernames, hostnames, and network addresses.

## Scope and method

The log contained approximately 22,400 events. The notebook selected one example for each of 20 distinct event IDs and sent those 20 entries to the class Qwen Coder model (`qwen3-coder-next`). These results describe the selected examples, not the entire log. The model's labels are triage suggestions and have not been independently confirmed as incidents.

## Model results for the 20 sampled entries

| Label | Count |
| --- | ---: |
| BENIGN | 17 |
| SUSPICIOUS | 3 |
| MALICIOUS | 0 |
| UNKNOWN | 0 |

## Findings requiring review

1. **Security event source unregistered:** The model flagged an event involving the Volume Shadow Copy service and an audit event source. A nearby sample showed that same source being registered. Review their sequence, service context, and related events before treating this as audit tampering.
2. **Blank-password query:** The model flagged a query concerning a built-in system account. Check the caller, timing, and neighboring account-management events to distinguish administration from reconnaissance.
3. **Cryptographic-key export:** The model flagged export of a Chrome test key. Review the key purpose, process, and related key deletion event before inferring exfiltration.

## Assessment

The sample produced three leads for manual investigation and no model-classified malicious events. It does **not** establish that the computer is uncompromised or that the three flagged events are attacks. The selection strategy can miss repeated failures, bursts, and sequences because it takes only one event per event ID. A stronger next pass would count all event IDs, inspect event timelines around the three leads, and prioritize repeated failed logons or account changes.

## Privacy

The raw Security CSV and the class API key are excluded from the public repository. Only this sanitized report and the credential-free notebook should be published.

