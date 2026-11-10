# Build Triage

Build Triage turns failed CI logs into a diagnosis with original evidence lines and concrete next steps. It runs locally and sends inference requests to Mozilla's hosted Otari gateway. The app requires Python 3.10 or later and uses only the standard library.

## Run on Windows

1. Sign in at [Otari.ai](https://otari.ai/), open your workspace, and choose **API Keys → Create Key**.
2. In **Models**, choose an open-weight model your workspace can call. Click **Use this model** and copy the exact model ID.
3. Open PowerShell in this repository and run the following. The key prompt masks input, and the settings apply to the current PowerShell session.

```powershell
$env:OTARI_MODEL = Read-Host 'Otari model ID'
$env:OTARI_API_KEY = [System.Net.NetworkCredential]::new('', (Read-Host 'Otari API Key' -AsSecureString)).Password
python app.py
```

For the route verified during the hackathon, add an OpenRouter provider key in your Otari organization and use `openrouter:liquid/lfm-2.5-2.6b:free` as the model ID. This route uses the OpenRouter credential stored in Otari. The app reads an Otari API key at startup.

If the Otari request example uses `eu.api.otari.ai`, run `$env:OTARI_REGION = 'eu'` before starting Python. The default is the global gateway. See [Otari Quickstart](https://docs.otari.ai/getting-started/quickstart.md).

4. Open [Build Triage](http://127.0.0.1:8765/). Paste an excerpt or click **填入示例日志** (Load sample), choose the output language, then click **分析日志** (Analyze). The result shows the failed stage, likely cause, evidence lines, and next steps. In Otari, check the workspace's **Usage** view for the request.

## Hackathon demo

Load the synthetic Maven sample and choose **English** for the demo. The verified run identified a package repository authentication failure and cited the original line 7, which reports `HTTP 401 Unauthorized`. Compare the cited lines with the input, then show the corresponding request in Otari Usage. OpenRouter's free tier can rate-limit requests, and model suggestions may contain unsupported details. The app retrieves evidence text by line number from the original input.

The app sends the submitted excerpt to Otari and the selected model service. Remove secrets and internal details before analyzing your own logs. The app keeps submitted logs in memory for the request.
