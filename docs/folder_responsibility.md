
---

## 📄 `folder_responsibility.md`

```md
# 📁 SPECTRA — Folder Responsibility Contract

> **Status:** Locked  
> **Purpose:** Prevent cross-layer contamination  
> **Applies To:** Entire repository

---

## 📌 Overview

This document defines **exact ownership, allowed content, and forbidden content** for every top-level folder in the SPECTRA repository.

Any violation is considered an **architectural defect**.

---

## 🧩 Root-Level Folders

---

### 📂 `backend/`

**Responsibility**
- All backend computation and orchestration

**Contains**
- FastAPI app
- Inference pipelines
- Preprocessing logic
- Model loading
- Report generation

**Must NOT Contain**
- Frontend code
- Training loops
- Dataset downloads
- Experiment notebooks

---

### 📂 `frontend/`

**Responsibility**
- Desktop UI (Electron + React)

**Contains**
- UI components
- API calls
- Visualization logic

**Must NOT Contain**
- AI models
- Python code
- Medical preprocessing
- File system logic

---

### 📂 `models/`

**Responsibility**
- Frozen, trained model artifacts

**Contains**
- `gatekeeper.pt`
- `brain.pt`
- `chest.pt`
- `bone.pt`

**Must NOT Contain**
- Training code
- Datasets
- Temporary checkpoints

---

### 📂 `data/`

#### `data/raw/`
- Local datasets only
- Never committed
- Never used at runtime

#### `data/processed/`
- Deterministic preprocessing outputs
- Matches inference preprocessing
- Small samples allowed

---

### 📂 `scripts/`

**Responsibility**
- Utility scripts
- Dataset conversion
- Validation helpers

**Rules**
- Never imported by backend runtime

---

### 📂 `notebooks/`

**Responsibility**
- Training & experimentation
- Google Colab workflows

**Rules**
- Offline / research only
- No runtime dependency

---

### 📂 `reports/`

**Responsibility**
- Generated outputs
- PDFs
- Annotated images

**Rules**
- Safe to delete
- Generated dynamically

---

### 📂 `docs/`

**Responsibility**
- System documentation

**Contains**
- Architecture rules
- API contracts
- Data manifests
- Methodology

---

## 🔒 Global Enforcement Rules

- ❌ No dataset files in Git
- ❌ No training inside backend
- ❌ No inference inside frontend
- ✅ `.gitkeep` allowed for empty folders

---

## ✅ Verification Checklist

Before pushing code, ask:
- Does this file belong here?
- Is this folder allowed to know about this logic?
- Would moving this break a rule?

If unsure → **STOP and redesign**.

---