# PlaceNet AI

PlaceNet AI helps placement teams organize resumes and find candidates for a role. It combines resume parsing, candidate search and ranking, hiring profiles, analytics, and an AI chat assistant.

## Features

- Parse resumes and manage a searchable candidate library.
- Create hiring profiles and rank candidates against role requirements.
- Search candidates and ask questions through an AI assistant.
- Review hiring analytics and export candidate results.

## Technology

- **Frontend:** Next.js, React, and TypeScript
- **Backend:** FastAPI and Python
- **Data:** SQLite and ChromaDB
- **Authentication and file storage:** Supabase
- **AI:** Configurable language-model provider; Gemini is the default

## Requirements

- Node.js 20.9 or later and npm
- Python 3.10 or later
- A Supabase project
- A Gemini API key, or credentials for another supported provider
- Tesseract OCR only if you need to process scanned PDFs

## Set up

Clone the repository and open it:

```bash
git clone https://github.com/drishtiisharma/PlaceNet-AI.git
cd PlaceNet-AI
```

### Configure environment variables

Copy the example files:

```bash
cp frontend/.env.example frontend/.env.local
cp backend/.env.example backend/.env
```

Fill in the values before starting the applications. The frontend needs the Supabase project URL and its public anon key. The backend needs the same project URL, a Supabase server key, and an AI provider key. Create a Supabase Storage bucket named `Resumes`, or set `SUPABASE_BUCKET` to the bucket you use.

Keep `SUPABASE_SECRET_KEY` and AI provider keys on the backend. Do not put server keys in variables prefixed with `NEXT_PUBLIC_` or commit real credentials.

| Variable | Application | Purpose |
| --- | --- | --- |
| `NEXT_PUBLIC_SUPABASE_URL` | Frontend | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Frontend | Supabase public anon key |
| `NEXT_PUBLIC_API_URL` | Frontend | Backend API URL; defaults to `http://127.0.0.1:8000` |
| `SUPABASE_URL` | Backend | Supabase project URL |
| `SUPABASE_SECRET_KEY` | Backend | Supabase server-side key for auth and storage |
| `SUPABASE_BUCKET` | Backend | Resume storage bucket; defaults to `Resumes` |
| `LLM_PROVIDER` | Backend | AI provider; defaults to `gemini` |
| `LLM_MODEL` | Backend | AI model; defaults to `gemini-2.5-flash` |
| `GEMINI_API_KEY` | Backend | Gemini API key when using the default provider |
| `TESSERACT_CMD` | Backend | Optional path to Tesseract when it is not on `PATH` |

For another AI provider, set `LLM_PROVIDER`, `LLM_MODEL`, and the matching provider key in `backend/.env`. The backend also supports numbered fallback providers; see `backend/app/ai/client.py` for the provider names and configuration.

### Install and run

Run the frontend and backend in separate terminals from the repository root.

In the first terminal:

```bash
cd frontend
npm ci
npm run dev
```

In the second terminal:

```bash
cd backend
python -m venv .venv
```

Activate the virtual environment, then install dependencies and start the API:

```powershell
# Windows PowerShell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app.main:app --reload
```

On macOS or Linux, activate with `source .venv/bin/activate` instead. The frontend is available at [http://localhost:3000](http://localhost:3000), and the FastAPI documentation is at [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs).

## Project structure

```text
PlaceNet-AI/
├── frontend/
│   ├── app/                 # Pages and routes
│   ├── components/          # UI and feature components
│   ├── hooks/               # React hooks
│   ├── lib/                 # API and Supabase clients
│   └── public/              # Static assets
├── backend/
│   ├── app/
│   │   ├── ai/              # AI provider and RAG logic
│   │   ├── api/             # FastAPI routes
│   │   ├── database/        # SQLite models and connection
│   │   ├── services/        # Resume, ranking, and analytics logic
│   │   └── schemas/         # API data models
│   ├── tests/               # Backend tests
│   └── requirements.txt     # Python dependencies
└── README.md
```

## Checks

Run these commands from their respective folders:

```bash
# frontend/
npm run lint

# backend/
pytest tests
```

## Troubleshooting

- **The frontend cannot reach the API:** Start the backend and check `NEXT_PUBLIC_API_URL`.
- **Supabase configuration errors:** Confirm the project URL and keys in both environment files. Check that the `Resumes` bucket exists for resume uploads.
- **AI requests fail:** Check that the selected provider, model, and API key are set in `backend/.env`.
- **Scanned PDF parsing fails:** Install Tesseract OCR and add it to `PATH`, or set `TESSERACT_CMD`.
