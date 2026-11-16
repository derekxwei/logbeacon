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

