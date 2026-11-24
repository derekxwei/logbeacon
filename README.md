# LogBeacon

LogBeacon is a Jupyter notebook that uses an OpenAI-compatible Qwen Coder endpoint to triage sampled Windows Security events into BENIGN, SUSPICIOUS, MALICIOUS, or UNKNOWN with a short reason. The class run used `qwen3-coder-next`; the resulting [sanitized assessment](assessment.md) describes 20 sampled events.

## Run

1. Open `LogBeacon.ipynb` in JupyterLab with Python 3.
2. Keep your Windows Security CSV private and upload it beside the notebook as `security_log.csv`.
3. Run the cells in order. Enter the HTTPS GPUStack `/v1` URL and API key only at the notebook's runtime prompts.
4. Review each model label against the original events before treating it as a security finding. The notebook writes `assessment.md`.

The notebook samples one entry from each of up to 20 distinct event IDs. It does not assess every event or establish that a host is safe. Model output is untrusted: malformed responses receive an UNKNOWN label.

## Security

Do not commit the API key, raw CSV, or unreviewed notebook outputs. Before publishing a notebook, clear outputs and check it for secrets and private event details. The included report is a sanitized summary of the class run.

---

## Lab 6

LogBeacon is a Jupyter notebook for preliminary security log triage with a class Qwen Coder endpoint. It reads up to 200 entries from a chosen text dataset, asks the model for a JSON label, severity, and reason, saves CSV and JSON results, and requests one Sigma detection rule from suspicious findings. Model labels and generated rules require analyst review.

## Run

1. Use Python 3.9+ and JupyterLab. Open `LogBeacon_Lab6.ipynb` and run the install cell.
2. Place `ssh_auth_log.txt` beside the notebook, or change `LOG_FILE` to another authorized text dataset.
3. Run the cells in order. Enter the instructor's HTTPS GPUStack URL ending in `/v1` and your API key at the runtime prompts. You can instead set `QWEN_BASE_URL` and `QWEN_API_KEY` in the environment. Never save the key in the notebook or repository.
4. Check `triage_results.csv`, `triage_results.json`, and `detection_rule.txt`. Review the classifications against the source logs and validate the Sigma rule before operational use.

The final notebook cell tests JSON parsing, invalid responses, input validation, and invalid severity values without a model connection. The live batch uses at most four concurrent requests to limit load on the shared class server.

## Observed lab run

On November 24, 2026, the notebook connected to `qwen3-coder-next` through the class endpoint and assessed 200 lines from the supplied `ssh_auth_log.txt`: 120 BENIGN, 70 SUSPICIOUS, and 10 MALICIOUS. The generated experimental Sigma rule is in `detection_rule.yml`; `results_summary.json` records the observed counts. The full CSV and JSON results were created in JupyterLab, but the browser's organizational download policy blocked exporting them to this repository.

The prompt injection example returned SUSPICIOUS with severity 3 under both the naive and guarded prompts. This single trial did not demonstrate that the guard changed the model's behavior. The four offline checks passed in the live notebook.

## Limits and security

The model may misclassify entries or obey instructions hidden in log text. Delimiting log text and validating output reduce risk but do not prove resistance to prompt injection. Do not upload private logs, API keys, or unreviewed notebook outputs to a public repository. This notebook does not deploy its generated detection rule.

## License

MIT. See `LICENSE`.
