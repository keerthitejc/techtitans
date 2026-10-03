InternLens - Internship Assistant (React + FastAPI)
Evidence-based internship matching: login, resume or skill input, hybrid search over 6,000+ notices,
cited fit explanations, missing-requirement notes, and deadline warnings.
Structure
```
internlens/
├── backend/                FastAPI API
│   ├── app/
│   │   ├── main.py         routes (auth, resume, search, saved)
│   │   ├── auth.py         SQLite users + JWT (PBKDF2 password hashing)
│   │   ├── data.py         loads the CSV, builds the TF-IDF index
│   │   ├── matching.py     skill evidence + hybrid scoring + eligibility + citations
│   │   ├── skills.py       skill taxonomy, aliases, related-skill clusters
│   │   └── resume.py       PDF / DOCX / TXT text extraction
│   ├── data/               internship_notices_raw.csv, student_profiles.csv, test_queries.csv
│   └── requirements.txt
└── frontend/               React (Vite)
    └── src/
        ├── App.jsx, api.js, utils.js, styles.css, main.jsx
        └── components/     Auth, Dashboard, SkillInput, ResumeUpload, ResultCard
```
Run it (two terminals, VS Code)
Backend (Python 3.10+):
```
cd backend
python -m venv .venv
.venv\Scripts\activate          # Windows   (macOS/Linux: source .venv/bin/activate)
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```
Frontend (Node 18+):
```
cd frontend
npm install
npm run dev
```
Open http://localhost:5173, create an account, then add skills or upload a resume.
API docs: http://localhost:8000/docs
Notes
Set `INTERNLENS_SECRET` to a long random string before deploying.
"Keyword only" in the dashboard disables related-skill matching and TF-IDF, so you can compare it with hybrid search for your evaluation.
Scanned resumes need OCR (Tesseract); text-based PDFs, DOCX and TXT work as is.
Upgrade path: replace TF-IDF in `data.py` with BGE embeddings + FAISS, and add the LightGBM ranker in `matching.py`.
The dataset is synthetic. Test on some real notices before reporting final results.
