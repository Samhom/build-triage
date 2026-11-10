# Build Triage

Build Triage turns failed CI logs into a diagnosis with original evidence lines and concrete next steps. It runs locally and uses the hosted Otari gateway for inference. The app requires Python 3.10 or later and has no third-party Python dependencies.

## Run on Windows

1. Sign in at [Otari.ai](https://otari.ai/), open your workspace, and choose **API Keys → Create Key**.
2. In **Models**, choose an open-weight model your workspace can call. Click **Use this model** and copy the exact model ID.
3. Open PowerShell in this repository and run the following. The key prompt masks input, and the settings apply to the current PowerShell session.

```powershell
$env:OTARI_MODEL = Read-Host 'Otari model ID'
$env:OTARI_API_KEY = [System.Net.NetworkCredential]::new('', (Read-Host 'Otari API Key' -AsSecureString)).Password
python app.py
```

If the Otari request example uses `eu.api.otari.ai`, run `$env:OTARI_REGION = 'eu'` before starting Python. The default is the global gateway. See [Otari Quickstart](https://docs.otari.ai/getting-started/quickstart.md).

4. Open [Build Triage](http://127.0.0.1:8765/). Paste an excerpt or click **填入示例日志**, then click **分析日志**. The result shows the failed stage, likely cause, evidence lines, and next steps. In Otari, check the workspace's **Usage** view for the request.

The bundled sample is synthetic. The app sends the submitted excerpt to Otari and the selected model service. Remove secrets and internal details before analyzing your own logs. The app does not write submitted logs to disk. It takes evidence text directly from the original lines by number, while the cause and next steps are model suggestions.
