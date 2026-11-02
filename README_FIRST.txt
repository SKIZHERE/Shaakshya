# Saakshya AI Starter (Windows)

This starter connects the Saakshya frontend to a Python FastAPI backend and the OpenAI Responses API.

## Before you begin

- Install Python 3.10 or newer.
- Install Visual Studio Code.
- Have an OpenAI API key and API billing enabled.
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

Open `backend/.env` in VS Code. Replace `PASTE_YOUR_SECRET_KEY_HERE` with your full secret key. Keep the key private. The model is set to `gpt-5.5` as an example; if your API project does not have access to it, use a model ID available to your project.

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

Go to Case Analyzer, paste a made-up test message, and click Analyse pasted text. The frontend sends text to your local backend, which calls OpenAI.

Example synthetic test:
"On 2 November 2026, account @sample_demo sent: 'Pay me 2000 rupees by tonight or I will share your private photos.'"

## 5. Stop the servers

Press Ctrl+C in each terminal.

## Troubleshooting

- `OPENAI_API_KEY is not configured`: check that `backend/.env` exists and contains the key.
- `OpenAI request failed`: check API billing, key status, key permissions, and model access.
- Browser network/CORS error: ensure both servers are running at the exact localhost ports above.
- `ModuleNotFoundError`: activate `.venv` and run `python -m pip install -r requirements.txt` again.
- Model not found / access denied: set `OPENAI_MODEL` in `.env` to a model ID available to your API project, then restart the backend.

## Important limits

- This starter sends pasted text to OpenAI only when the user clicks Analyze. It does not upload the evidence files.
- OCR, image upload to the model, authentication, database, encrypted storage, trusted timestamps, legal certification, and government/platform integrations are not implemented.
- CORS is configured for local development only. Do not deploy this as-is.
- A matching source quote does not prove the AI's interpretation is correct. Every result needs human review.
- `store=False` is set on the Responses API request, but this alone is not a guarantee of zero data retention. Review the provider's current data controls and your legal/privacy obligations before processing sensitive content.
