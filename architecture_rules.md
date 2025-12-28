# 🧠 SPECTRA — Architecture Rules & Enforcement Contract

> **Status:** Authoritative  
> **Applies To:** All contributors  
> **Last Modified:** Phase 1  
> **Owner:** System Architecture (M1)

---

## 📌 Document Purpose

This document defines **non-negotiable architectural rules** for the **SPECTRA** system.

Its purpose is to:

- Prevent architectural drift  
- Eliminate cross-layer contamination  
- Ensure clean packaging into a single executable  
- Guarantee long-term maintainability  

⚠️ **Any violation of these rules is considered a blocking defect.**

---

## 🏗️ 1. Architectural Philosophy

SPECTRA follows a **strict layered architecture**.

Each layer has:

- A **single responsibility**
- Explicit **allowed dependencies**
- Explicit **forbidden dependencies**

> **Rule:**  
> If code does not clearly belong to one layer, it must not exist.

---

## 🧩 2. Top-Level Layer Definitions

---

### 🎨 2.1 Frontend Layer (`frontend/`)

**Purpose**
- User interaction
- File upload
- Visualization of AI output
- Report download

**Allowed Technologies**
- Electron  
- React  
- JavaScript / TypeScript  
- HTML / CSS  

**Allowed Actions**
- Send HTTP requests to backend
- Render images and PDFs
- Display error messages
- Handle UI state

**Explicitly Forbidden**
- ❌ AI model loading  
- ❌ PyTorch / MONAI / NumPy  
- ❌ Medical preprocessing logic  
- ❌ Direct filesystem access to datasets  
- ❌ Training or inference logic  

> 🔒 **Rule:**  
> The frontend must be completely **AI-agnostic**.

---

### ⚙️ 2.2 Backend Layer (`backend/`)

**Purpose**
- Input normalization
- AI model orchestration
- Inference execution
- Post-processing
- Report generation

**Allowed Technologies**
- Python  
- FastAPI  
- PyTorch (**inference only**)  
- MONAI (**inference only**)  
- NumPy / OpenCV  
- Report generation libraries  

**Allowed Actions**
- Load trained `.pt` models
- Run deterministic preprocessing
- Perform inference
- Generate overlays and reports
- Return structured JSON responses

**Explicitly Forbidden**
- ❌ Model training loops  
- ❌ Dataset downloading  
- ❌ Randomized preprocessing at inference  
- ❌ UI rendering logic  
- ❌ Frontend framework code  

> 🔒 **Rule:**  
> Backend = **pure computation and orchestration**, never training or UI.

---

### 🧠 2.3 Model Artifacts (`models/`)

**Purpose**
- Store **trained, frozen model weights only**

**Allowed Files**
```text
gatekeeper.pt
brain.pt
chest.pt
bone.pt