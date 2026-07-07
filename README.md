# DarkWeb Scrapper

A full-stack dark web intelligence and monitoring platform built with a Python FastAPI backend and a React + Vite frontend.

This repository is now maintained by `chandu-108` and includes the main scan, monitor, alert, history, and analytics workflows.

## Overview

DarkWeb Scrapper scans surface web and dark web URLs, extracts intelligence signals, computes risk scores, tracks history, and surfaces alerts for changes in threat level.

Key capabilities:
- URL status detection for online/offline/timeouts/errors
- Threat score and risk classification
- Category classification and content metadata
- IOC extraction from page text and files
- Scheduled monitoring and alert generation
- Scan history, comparison, analytics, and notifications

## What’s Included

Backend:
- `backend/main.py` starts the FastAPI app
- `backend/app/api/routes` defines HTTP endpoints
- `backend/app/services` handles scanning, monitoring, alerts, and history
- `backend/app/scraper.py`, `parser.py`, `analyzer.py`, `file_analyzer.py` extract and analyze content
- `backend/app/tor_proxy.py` manages Tor/SOCKS requests for `.onion` access
- `backend/app/database.py` manages MongoDB persistence

Frontend:
- `frontend/src/App.jsx` defines routes and layout
- Pages for `Dashboard`, `Monitors`, `History`, `Alerts`, and `Analytics`
- `frontend/src/services/api.js` centralizes API requests
- Charts and UI components render scan results, alerts, and trends

## Tech Stack

Backend:
- Python, FastAPI, Uvicorn
- Pydantic, Requests, BeautifulSoup
- MongoDB via `pymongo`
- APScheduler for scheduled monitor jobs
- Tor/SOCKS proxy for hidden service access

Frontend:
- React, Vite
- Axios for API calls
- Recharts for data visualization

## Quick Start

1. Create and activate a virtual environment:
```bash
python -m venv .venv
.venv\Scripts\activate
```

2. Install backend dependencies:
```bash
pip install -r backend/requirements.txt
```

3. Configure backend environment variables in `backend/.env`:
```env
MONGODB_URI=mongodb+srv://<user>:<pass>@<cluster>.mongodb.net/darkweb_scrapper?appName=darkwebCluster
DATABASE_NAME=darkweb_scrapper
COLLECTION_NAME=scraped_data
HOST=0.0.0.0
PORT=8000
TOR_PROXY_HTTP=socks5h://127.0.0.1:9050
TOR_PROXY_HTTPS=socks5h://127.0.0.1:9050
```

4. Install frontend dependencies:
```bash
cd frontend
npm install
```

5. Start the backend:
```bash
cd ..
.venv\Scripts\activate
python backend/main.py
```

6. Start the frontend:
```bash
cd frontend
npm run dev
```

7. Open the application:
- Frontend: `http://localhost:5173`
- API docs: `http://localhost:8000/docs`

## Important Notes

- Tor must be running and accessible on `127.0.0.1:9050` for `.onion` scans.
- MongoDB must be reachable before running scans.
- Use the frontend dashboard to create monitors, run scans, and view alerts.

## Project Structure

- `backend/` — FastAPI service, services, data processing, database integration
- `frontend/` — React/Vite UI and visualization
- `data/` — sample and downloaded datasets

## Verification

Check backend health:
```bash
curl http://localhost:8000/health
```

If it returns a valid health response, the backend is running correctly.

---

Maintained and updated by `chandu-108`.

{"status":"ok"}
```

Scan test:
```bash
curl -X POST http://localhost:8000/scan \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}'
```

Frontend checks:
- Dashboard loads without CORS errors
- Scan request returns cards/charts
- History page shows records after scan
- Monitors page creates and refreshes monitor list
- Alerts page updates when threat increase occurs

## 12) Logging and Observability

Generated logs:
- `logs/system.log` (all events)
- `logs/alerts.log` (warning/error/critical focus)

Common startup messages:
- MongoDB connected
- monitor manager initialized
- API startup banner

## 13) Troubleshooting Guide

If `/scan` fails with connection errors:
- Verify Tor is running on `127.0.0.1:9050` for `.onion` targets.
- Verify `MONGODB_URI` is correct.

If monitor endpoints fail:
- Ensure `apscheduler` is installed.

If scan returns low data:
- Target may block requests or return non-text content.
- Parser intentionally rejects clear binary content types.

If file analysis is missing:
- Install `clamscan`, `binwalk`, and `exiftool`.
- System still runs without them; result includes tool status/errors.

If frontend cannot reach backend:
- Check `VITE_API_BASE_URL`.
- Check backend host/port and CORS settings.

## 14) Current Implementation Notes

- `.onion` URLs require Tor.
- Content change raises stored threat score by +15 (capped at 100).
- Monitor alerts are triggered when latest threat score exceeds previous score.
- History endpoint returns latest scan per URL for compact view.
- Monitor delete route currently shares decorators with pause handler in `backend/app/api/routes/monitors.py`; if DELETE `/monitors/{monitor_id}` does not remove a monitor in your environment, call `DELETE /monitors/all` or patch the route mapping.

## 15) Security and Legal Notice

This project is for research and defensive threat intelligence workflows.

You must:
- follow applicable law,
- only monitor targets you are authorized to access,
- avoid illegal or harmful use.

## 16) Quick File Index

- Backend entry: `backend/main.py`
- Frontend entry: `frontend/src/main.jsx`
- Router shell: `frontend/src/App.jsx`
- API client: `frontend/src/services/api.js`
- Scan orchestration: `backend/app/services/scan_service.py`
- Monitor scheduler: `backend/app/monitor.py`
- DB layer: `backend/app/database.py`
- Scraper: `backend/app/scraper.py`
- Parser: `backend/app/parser.py`
- Analyzer: `backend/app/analyzer.py`
- File downloader: `backend/app/downloader.py`
- File analyzer: `backend/app/file_analyzer.py`
- Config: `backend/app/core/config.py`
- Logger: `backend/app/logger.py`

---

If you want, the next step is I can also generate a matching `ARCHITECTURE.md` with sequence diagrams for `/scan`, monitor scheduler cycles, and alerts lifecycle.
