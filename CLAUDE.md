# Fair Fix - Invoice Analysis System

## Project Overview
Fair Fix to system analizy kosztów części samochodowych poprzez skanowanie faktur i dokumentów używający Google Document AI.

**Faza 1 (MVP - obecna):** Document Processing & Data Extraction
- Skanowanie faktur (PDF, zdjęcia, dokumenty pisane ręcznie)
- Wyodrębnianie pozycji: nazwy części, ilości, ceny brutto/netto
- Rozpoznawanie danych pojazdu: marka, model, silnik, rocznik, VIN
- Zwracanie strukturalizowanych danych

**Faza 2 (przyszłość):** Cost Analysis & Price Comparison
- Połączenie z bazą danych części
- Porównywanie cen z rynkiem
- Generowanie raportów oszczędności

## Tech Stack
- **Frontend:** Next.js 14 + TypeScript + Tailwind CSS
- **Backend:** Python 3.11 + FastAPI + Pydantic
- **Document Processing:** Google Document AI
- **Development:** Claude Code + VS Code

## Project Structure
```
fair-fix/
├── frontend/              # Next.js React application
│   ├── src/
│   │   ├── app/          # Next.js App Router pages
│   │   ├── components/   # Reusable UI components
│   │   ├── lib/          # Utilities & API calls
│   │   └── types/        # TypeScript type definitions
│   └── package.json
├── backend/               # FastAPI Python application
│   ├── app/
│   │   ├── routers/      # API route handlers
│   │   ├── services/     # Business logic (Document AI)
│   │   ├── models/       # Pydantic data models
│   │   ├── utils/        # Helper functions
│   │   └── core/         # Configuration & settings
│   └── requirements.txt
├── docs/                  # Project documentation
├── CLAUDE.md             # Development commands & context
└── README.md             # Project overview
```

## Development Commands

### Backend Commands (Python/FastAPI)
```bash
# Setup virtual environment
python -m venv venv
source venv/bin/activate  # macOS/Linux
# venv\Scripts\activate   # Windows

# Install dependencies
pip install -r backend/requirements.txt

# Start development server
cd backend && uvicorn app.main:app --reload --port 8000

# Run tests
cd backend && pytest

# Lint code
cd backend && black . && isort . && flake8
```

### Frontend Commands (Next.js)
```bash
# Install dependencies
cd frontend && npm install

# Start development server
npm run dev  # runs on port 3000

# Build for production
npm run build

# Run tests
npm run test

# Lint code
npm run lint
```

### Full Stack Development
```bash
# Start both frontend and backend simultaneously
npm run dev:full

# Build entire project
npm run build:all

# Run all tests
npm run test:all

# Lint entire codebase
npm run lint:all
```

## Google Cloud Setup
1. **Project Configuration:**
   - GCP Project z włączonym Document AI API
   - Service Account Key w `/backend/google-cloud-key.json`
   - Environment variables skonfigurowane w `.env`

2. **Document AI Processor:**
   - Form Parser dla faktur
   - Custom Document Extractor (opcjonalnie)

## Environment Variables
```
# Backend (.env)
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_APPLICATION_CREDENTIALS=./google-cloud-key.json
DOCUMENT_AI_PROCESSOR_ID=your-processor-id
DOCUMENT_AI_LOCATION=us
CORS_ORIGINS=http://localhost:3000

# Frontend (.env.local)
NEXT_PUBLIC_API_URL=http://localhost:8000
```

## Current MVP Goals
1. ✅ Project setup & structure
2. 🔄 Document upload interface
3. 🔄 Google Document AI integration
4. 🔄 Data extraction & parsing
5. 🔄 Results display & formatting

## Next Steps After MVP
- Database integration (PostgreSQL/MongoDB)
- Parts catalog & price comparison
- User authentication
- Savings analysis & reporting
- Public domain deployment