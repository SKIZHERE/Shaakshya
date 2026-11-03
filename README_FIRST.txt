# Saakshya AI Starter (Windows)

This starter connects the Saakshya frontend to a Python FastAPI backend and the Mistral API.

## Before you begin

- Install Python 3.10 or newer.
- Install Visual Studio Code.
- Have a Mistral API key and API billing enabled.
- Never share your API key or commit `backend/.env` to GitHub.
- Start with synthetic, made-up test messages only. Do not use real victim evidence in this prototype.

## 1. Open the project

1. Extract `saakshya_ai_starter.zip`.
2. Open the extracted `saakshya_ai_starter` folder in VS Code.
3. Open VS Code Terminal: Terminal > New Terminal.

## 2. Set up the backend

In the VS Code terminal:

```powershell
cd backend
py -m venv .venv
.venv\Scripts\activate
python -m pip install -r requirements.txt
copy .env.example .env
```

Open `backend/.env` in VS Code. Replace `PASTE_YOUR_SECRET_KEY_HERE` with your full Mistral API key. Keep the key private. The backend uses the configured Mistral agent by default. You can also set `MISTRAL_MODEL` and leave `MISTRAL_AGENT_ID` empty to call a model directly.

## 3. Start the backend

In the same terminal, while inside `backend`:

```powershell
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Keep this terminal running. Visit http://127.0.0.1:8000/ and expect a JSON status response.

## 4. Start the website

Open a SECOND VS Code terminal. From the project root:

```powershell
cd frontend
py -m http.server 5500
```

Open http://127.0.0.1:5500/ in Chrome.

Select **Explore demo workspace**, go to Case Analyzer, paste a made-up test message, and click Analyse pasted text. The frontend sends text to your local backend, which calls Mistral.

Example synthetic test:
"On 2 November 2026, account @sample_demo sent: 'Pay me 2000 rupees by tonight or I will share your private photos.'"

## 5. Stop the servers

Press Ctrl+C in each terminal.

## Deploying on Vercel

Add the secrets in the Vercel dashboard, not in the repository:

1. Open your project on vercel.com.
2. Go to **Settings > Environment Variables**.
3. Add `MISTRAL_API_KEY` with your key for Production, Preview, and Development.
4. Add `MISTRAL_AGENT_ID` (default: `ag_019ba298aa457255a91731d5eac3c883`), `MISTRAL_AGENT_VERSION` (default: `1`), and `MISTRAL_MODEL` (default: `mistral-medium-latest`).
5. Save the variables, then redeploy the project so the function picks them up.

With the Vercel CLI you can also run:

```powershell
vercel env add MISTRAL_API_KEY production
vercel env add MISTRAL_AGENT_ID production
vercel env add MISTRAL_AGENT_VERSION production
vercel env add MISTRAL_MODEL production
vercel --prod
```

If you change an environment variable, redeploy. Vercel does not apply new values to an existing deployment automatically.

## Troubleshooting

- `MISTRAL_API_KEY is not configured`: check that `backend/.env` exists locally, or that the variable is set in Vercel and the project was redeployed.
- `Mistral request failed`: check API billing, key status, agent ID/version, model access, and backend logs.
- Browser network/CORS error: ensure both servers are running at the exact localhost ports above.
- `ModuleNotFoundError`: activate `.venv` and run `python -m pip install -r requirements.txt` again.
- Agent not found / access denied: check `MISTRAL_AGENT_ID` and `MISTRAL_AGENT_VERSION` in Mistral Studio, or clear `MISTRAL_AGENT_ID` to use `MISTRAL_MODEL` directly.

## Important limits

- This starter sends pasted text to Mistral only when the user clicks Analyse. It does not upload the evidence files.
- OCR, image upload to the model, authentication, database, encrypted storage, trusted timestamps, legal certification, and government/platform integrations are not implemented.
- CORS is configured for local development only. Do not deploy this as-is.
- A matching source quote does not prove the AI's interpretation is correct. Every result needs human review.
- `store=False` is set on the Mistral conversation request, but this alone is not a guarantee of zero data retention. Review the provider's current data controls and your legal/privacy obligations before processing sensitive content.
