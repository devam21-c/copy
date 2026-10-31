# Executive AI Chief of Staff

An enterprise-grade, agentic executive assistant built with **Gemini 3.8 Flash**, **FastAPI**, **MongoDB Vector Memory**, and a modern **React UI**.

---

## Architecture & Core Features

```
+───────────────────────────────────────────────────────────+
│              Modern React Executive Interface             │
│    (Real-time Chat, Memory Vault Inspector, Live Alerts)  │
+───────────────────────────────────────────────────────────+
                              │
               POST /api/chat │ GET /api/memories
                              ▼
+───────────────────────────────────────────────────────────+
│               FastAPI Agent Backend (Python)              │
│  1. Vector Retrieval (MongoDB $vectorSearch / Embeddings) │
│  2. Dynamic Memory Injection into System Instructions     │
│  3. Agentic Extraction Tool Call (Gemini 3.8 Flash)       │
+───────────────────────────────────────────────────────────+
              │                               │
              ▼                               ▼
+───────────────────────────+   +───────────────────────────+
│     Google GenAI SDK      │   │          MongoDB          │
│     - gemini-3.8-flash    │   │  - memories collection    │
│     - gemini-embedding    │   │  - TTL index (expiresAt)  │
│     - Tool Calling        │   │  - Vector embeddings      │
+───────────────────────────+   +───────────────────────────+
```

### 1. Vector Retrieval
Before generating an answer to any prompt, the backend automatically embeds the user's message using `gemini-embedding-001` and queries MongoDB's `memories` collection. Expired statuses are filtered out, and the most relevant preferences and constraints are injected directly into Gemini's system instructions.

### 2. Real-Time Memory Extraction
When the user states a preference (*"I prefer concise briefings"*), a temporary constraint (*"I'm out sick with flu today"*), or a fact, Gemini 3.8 Flash triggers the `extract_and_store_memory` tool. This commits the structured JSON document to MongoDB along with its vector embedding and TTL timestamp.

### 3. Immediate Usability
The React application is built into `frontend/dist` and served directly by FastAPI at `http://localhost:8000`.

---

## How to Run

### Option A: 1-Click Launch (Recommended)
Simply double-click [`start.bat`](file:///c:/Users/devam/project%20dbms/executive-ai-chief-of-staff/start.bat) or run in PowerShell:

```powershell
cd "c:\Users\devam\project dbms\executive-ai-chief-of-staff"
.\start.ps1
```

Then open your browser at **`http://localhost:8000`**.

### Option B: Running with uv
```powershell
cd "c:\Users\devam\project dbms\executive-ai-chief-of-staff"
& "$HOME\.local\bin\uv.exe" run uvicorn backend.main:app --port 8000 --reload
```

### Option C: Frontend Dev Server (Vite)
If you wish to run the Vite dev server separately:
```powershell
cd "c:\Users\devam\project dbms\executive-ai-chief-of-staff\frontend"
npm.cmd run dev
```
Open **`http://localhost:3000`** (it proxies all `/api` calls directly to FastAPI on port 8000).
