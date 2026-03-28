# Karaoke Stem Separator (React + Django + Postgres)

Full-stack project to upload a song, separate stems (vocals/instrumental), and export custom mixes.

## Tech Stack

- Frontend: React + Vite
- Backend: Django + Django REST Framework
- Database: Postgres
- Audio engine: Demucs + FFmpeg

## Features

- Upload audio file (`.mp3`, `.wav`, `.flac`)
- Run stem separation (Demucs)
- Download stems:
  - vocals
  - instrumental/music
  - drums/bass/other
- Create and download custom mix:
  - vocal + music
  - vocal + instrumental
  - any selected stems

## 1) Start Postgres

```powershell
cd infra
docker compose up -d
```

## 2) Backend setup (Django)

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
python manage.py migrate
python manage.py runserver
```

`demucs` is listed in `backend/requirements.txt`. FFmpeg must be installed and available on `PATH` (Demucs uses it).

## 3) Frontend setup (React)

```powershell
cd frontend
npm install
copy .env.example .env
npm run dev
```

## API Endpoints

- `GET /api/health/`
- `POST /api/jobs/` - upload and process one file
- `GET /api/jobs/{job_id}/` - job status
- `GET /api/jobs/{job_id}/download/{stem}/` - stem download
- `POST /api/jobs/{job_id}/mix/` - custom mix download

## Notes

- Current version processes one request synchronously in API call.
- For production, add background queue (Celery/RQ), auth, and rate limits.
