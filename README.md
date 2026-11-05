# Website Vulnerability Scanner — AI Experiment

A Flask-based website security checker with an experimental AI-assisted explanation endpoint. This repository contains an earlier iteration of the project; the application source is stored under `website_vulnerability_scanner-main/`.

## What it checks

The scanner makes basic, passive requests to a target after the user confirms they are authorised to assess it. The current implementation includes:

- HTTPS usage and a best-effort TLS certificate expiry check
- Common HTTP security headers: HSTS, Content Security Policy, X-Content-Type-Options, clickjacking protection and Referrer-Policy
- Cookie `Secure` and `HttpOnly` flags
- The presence and contents of `robots.txt`
- The HTTP `Server` response header
- A simple severity-weighted risk score and summary counts

The experimental `/explain_fix` endpoint is intended to turn a finding and its suggested remediation into plain-language guidance using Google's Generative AI SDK. A fallback explanation is returned when the model is unavailable.

## Project structure

```text
website_vulnerability_scanner-main/
├── app.py
├── requirements.txt
├── templates/
└── testsite/
```

## Run locally

Use Python 3.10+ (or a compatible Python version), then:

```bash
git clone https://github.com/akarshgshankar/Website-Vul-Scanner-with-ai.git
cd Website-Vul-Scanner-with-ai/website_vulnerability_scanner-main

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\\Scripts\\activate

pip install -r requirements.txt
pip install google-generativeai
python app.py
```

Open the local Flask address printed in the terminal.

### AI endpoint setup

The code imports Google's Generative AI SDK, but the API-key configuration line is currently commented out in `app.py`. Before expecting AI explanations to work, configure the SDK with an environment-provided key (never commit the key):

```python
genai.configure(api_key=os.getenv("GOOGLE_API_KEY"))
```

Set `GOOGLE_API_KEY` in your shell before starting the app. The application also uses the Google API Core exception package, which is normally installed as a dependency of the SDK.

## Important limitations

This is a learning project and a basic passive checker, **not** a comprehensive vulnerability scanner or a substitute for a professional security assessment.

- It inspects selected HTTP/TLS signals; it does not perform authenticated testing, exploit checks, fuzzing or full application crawling.
- A missing header is an indicator to review, not proof that a site is exploitable.
- The risk score is a simple heuristic, not a validated security rating.
- Only scan systems you own or have explicit permission to assess.
- The current host check is a simple local-address filter and should not be treated as production-grade SSRF protection.

## Tech

`Python` · `Flask` · `Requests` · `Google Generative AI (experimental)`

---

This is an earlier project iteration. The newer **[WebScan Ultra](https://github.com/akarshgshankar/Webscan-Ultra)** repository is the intended home for the consolidated project.
