---

# 📁 `folder_responsibility.md`

```md
# 📁 SPECTRA — Folder Responsibility Contract

> **Status:** Authoritative  
> **Audience:** All Developers  
> **Purpose:** Prevent misuse of directories

---

## 📌 Why This Document Exists

This document defines **what each folder is allowed and forbidden to contain**.

Violating these rules causes:
- Build failures
- Packaging issues
- Runtime bugs
- Long-term technical debt

---

## 🗂️ Root Structure Overview

spectra-ai/
├── backend/
├── frontend/
├── models/
├── data/
│ ├── raw/
│ └── processed/
├── scripts/
├── notebooks/
├── reports/
├── docs/

yaml
Copy code

---

## ⚙️ `backend/`

### Purpose
- Runtime inference engine
- FastAPI server
- Preprocessing & orchestration

### Allowed
- API routes
- Inference logic
- Deterministic preprocessing
- Model loading

### Forbidden
- ❌ Training code
- ❌ Dataset downloads
- ❌ UI logic
- ❌ Random augmentations

---

## 🎨 `frontend/`

### Purpose
- Desktop UI
- User interaction
- Visualization

### Allowed
- Electron main process
- React components
- API calls
- Image/PDF rendering

### Forbidden
- ❌ AI logic
- ❌ Python code
- ❌ Model weights
- ❌ Medical preprocessing

---

## 🧠 `models/`

### Purpose
- Store trained model artifacts

### Allowed
```text
gatekeeper.pt
brain.pt
chest.pt
bone.pt
Forbidden
❌ Training checkpoints

❌ Optimizer states

❌ Raw data

❌ Scripts

📊 data/raw/
Purpose
Temporary local dataset storage

Rules
❌ Never committed

❌ Never accessed at runtime

❌ Exists only locally

📈 data/processed/
Purpose
Deterministic preprocessing outputs

Validation samples

Rules
Must mirror inference preprocessing

Large files stay external

🛠️ scripts/
Purpose
One-time utilities

Dataset conversion

Validation helpers

Rules
❌ Never imported by backend

❌ Never required at runtime

📓 notebooks/
Purpose
Model training

Google Colab workflows

Rules
Training only

Experimental allowed

❌ No runtime dependency

📄 reports/
Purpose
Generated outputs

PDFs and overlays

Rules
Runtime-generated only

Safe to delete

Never committed long-term

📚 docs/
Purpose
Architecture

Contracts

Methodology

Required Files
architecture_rules.md

api_contract.md

folder_responsibility.md

🚫 Absolute Rules
❌ No datasets in Git

❌ No training in backend

❌ No AI in frontend

❌ No cross-layer imports

✅ Final Enforcement Rule
If a folder’s purpose is unclear, STOP and document before coding.