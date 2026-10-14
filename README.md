# Employee Attendance & Analytics API

A FastAPI backend with MongoDB to handle employee attendance, punches, corrections, and reporting analytics.

---

## Quick Setup & How to Run

### 1. Set up the virtual environment
```bash
python -m venv .venv
# On Windows:
.\.venv\Scripts\activate
# On Linux/macOS:
source .venv/bin/activate
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure Database
Set environment variables or create a `.env` file:
```env
MONGO_URI=mongodb://localhost:27017
MONGO_DB=attendance_db
```

### 4. Seed sample data (optional)
```bash
python sample_seed.py
```

### 5. Run the server
```bash
uvicorn app.main:app --port 8000
```
- Health check: `GET http://localhost:8000/health`
- Interactive API Docs (Swagger UI): `http://localhost:8000/docs`

---

## What was done

### 1. Part A: Starter Review (`REVIEW.md`)
- Reviewed `app/main.py` and fixed all initial defects (including race conditions on duplicate inserts, timezone handling in IST, edge cases with late calculation and overnight shifts, and banker's rounding).
- Documented findings in [`REVIEW.md`](REVIEW.md).

### 2. Part B: Missing Endpoints (`app/main.py`)
Implemented all required endpoints directly in `app/main.py`:
- **Punch-Out** (`POST /attendance/punch-out`): Calculates work hours, overtime, and half days.
- **Regularization** (`PATCH /attendance/{emp_code}/{date}`): Allows updating attendance records with an audit history.
- **Analytics Aggregations**:
  - `GET /analytics/employees/{emp_code}/monthly`: Monthly stats for a single employee.
  - `GET /analytics/departments/summary`: Department stats including employees with zero logs.
  - `GET /analytics/leaderboard/late`: Ranks late employees handling ties using MongoDB `$rank`.
  - `GET /analytics/departments/{department}/trend`: 7-day attendance trend using MongoDB `$densify` for gap filling.

### 3. Part C: Explain & Decisions (`DECISIONS.md`)
- Implemented `GET /admin/explain/{endpoint}` to prove queries use index scans (`IXSCAN`).
- Answered the 5 core architectural and indexing questions in [`DECISIONS.md`](DECISIONS.md).

---

## Submission Details
- All code is in `app/main.py`.
- No sensitive files (`.env`, `.venv`) are committed.
- Everything runs and passes all contract checks.
