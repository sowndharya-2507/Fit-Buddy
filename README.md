# FitBuddy — AI Fitness Plan Generator

FitBuddy is a FastAPI + Jinja2 + SQLite application that uses Google's Gemini API to generate a personalized 7-day workout plan, a nutrition/recovery tip, and AI revisions from user feedback.

## What is included

- Responsive HTML/Jinja2 frontend
- FastAPI web routes
- JSON API under `/api`
- SQLite + SQLAlchemy persistence
- Structured Gemini JSON output validated with Pydantic
- Original and updated plans retained in the database
- Optional admin token for `/view-all-users`
- Basic safety language and input validation
- Local development configuration
- API docs at `/docs`

## Architecture

```text
Browser
  │
  ├── Jinja2 HTML pages
  │
FastAPI
  ├── Web routes
  ├── JSON API
  │
  ├── AI layer ── Google Gemini API
  │
  └── SQLAlchemy ── SQLite
```

## Important update from the supplied project document

The supplied specification describes the older `google-generativeai` SDK and Gemini 1.5 Pro / Gemini Flash. This implementation uses Google's current `google-genai` SDK and configurable model names. The defaults are current stable Gemini models; set `WORKOUT_MODEL` and `NUTRITION_MODEL` in `.env` if your Google account/project exposes different models.

## VS Code setup

### 1. Open the project

Open the `FitBuddy` folder in VS Code.

### 2. Create a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Windows CMD:

```cmd
python -m venv .venv
.venv\Scripts\activate
```

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure Gemini

Copy `.env.example` to `.env` and put your Google AI Studio API key in:

```text
GOOGLE_API_KEY=...
```

Do not commit `.env` to Git.

### 5. Run

```bash
uvicorn app.main:app --reload
```

Open:

- App: http://127.0.0.1:8000
- API docs: http://127.0.0.1:8000/docs
- Admin view: http://127.0.0.1:8000/view-all-users

If `ADMIN_TOKEN` is set, use:

```text
http://127.0.0.1:8000/view-all-users?token=YOUR_TOKEN
```

## Testing

Run:

```bash
pytest -q
```

The tests validate the application structure and core schema behavior without requiring a live Gemini request.

## API examples

### Create a plan

`POST /api/plans`

```json
{
  "username": "Alex",
  "user_id": "alex-001",
  "age": 28,
  "weight": 72,
  "goal": "muscle gain",
  "intensity": "medium"
}
```

### Submit feedback

`POST /api/plans/feedback`

```json
{
  "user_id": "alex-001",
  "feedback": "Add more cardio and make Friday a recovery-focused day."
}
```

### Get latest plan

`GET /api/users/alex-001/latest`

## Notes

This application is an educational/general-wellness planner, not a medical or clinical system. AI-generated fitness content should be reviewed by the user and a qualified professional when appropriate, especially for injuries, medical conditions, pregnancy, or high-risk training.

For production deployment, add real authentication/authorization, HTTPS, database migrations, rate limiting, centralized logging, secret management, and background job handling for long-running AI requests.
