# SPECTRA Folder Responsibility Contract

Status: Authoritative  
Audience: All developers  
Scope: Entire repository

---

## 1. Purpose of This Document

This document defines the allowed and forbidden contents of every folder in the SPECTRA repository.

Its purpose is to:
- Prevent architectural violations
- Avoid cross-layer coupling
- Ensure clean packaging into a single executable
- Maintain long-term maintainability

Any deviation from these rules is considered a blocking architectural defect.

---

## 2. Repository Root Structure

The repository must follow this structure exactly:

spectra-ai/
- backend/
- frontend/
- models/
- data/
  - raw/
  - processed/
- scripts/
- notebooks/
- reports/
- docs/

No additional top-level folders may be introduced without architectural review.

---

## 3. backend/

### Purpose
The backend folder contains the runtime inference engine.

It is responsible for:
- Input normalization
- Model orchestration
- Inference execution
- Post-processing
- Report generation

### Allowed Contents
- FastAPI application code
- Inference-only PyTorch code
- Deterministic preprocessing logic
- Model loading logic
- PDF and overlay generation code

### Forbidden Contents
- Training loops
- Dataset download logic
- Random data augmentation
- Frontend or UI code
- Notebook files
- Experimental scripts

Rule:
The backend must never perform training or depend on training utilities.

---

## 4. frontend/

### Purpose
The frontend folder contains the desktop user interface.

It is responsible for:
- File upload
- User interaction
- Displaying results
- Downloading reports

### Allowed Contents
- Electron main process code
- React components
- UI state management
- API request logic
- Image and PDF rendering

### Forbidden Contents
- AI models
- Python code
- Medical preprocessing
- Dataset access
- Training or inference logic

Rule:
The frontend must remain completely AI-agnostic.

---

## 5. models/

### Purpose
The models folder stores trained, frozen model weights used at runtime.

### Allowed Contents
- gatekeeper.pt
- brain.pt
- chest.pt
- bone.pt

### Rules
- Models must be trained externally
- Models must be immutable at runtime
- Models must be versioned intentionally

### Forbidden Contents
- Raw datasets
- Training scripts
- Optimizer states
- Intermediate checkpoints

---

## 6. data/

### 6.1 data/raw/

Purpose:
Temporary local storage of original datasets.

Rules:
- Must never be committed to Git
- Must never be accessed by runtime backend
- Exists only on developer machines or external storage

---

### 6.2 data/processed/

Purpose:
Deterministic preprocessing outputs used for training validation and testing.

Rules:
- Must exactly match inference preprocessing
- May contain small representative samples
- Large datasets must remain external

---

## 7. scripts/

### Purpose
One-off utilities and helper scripts.

Examples:
- Dataset conversion
- Validation scripts
- Preprocessing verification tools

Rules:
- Must never be imported by backend runtime
- Must never be required for application execution

---

## 8. notebooks/

### Purpose
Training and experimentation workflows.

Examples:
- Google Colab notebooks
- Model training notebooks
- Data exploration notebooks

Rules:
- Training only
- Experimental code allowed
- Must never be used at runtime

---

## 9. reports/

### Purpose
Generated output artifacts.

Examples:
- PDF clinical reports
- Annotated images

Rules:
- Generated at runtime only
- Safe to delete and regenerate
- Not part of core logic

---

## 10. docs/

### Purpose
Project documentation and contracts.

Required documents:
- architecture_rules.md
- api_contract.md
- folder_responsibility.md

Rules:
- Documentation must reflect current architecture
- Any architectural change must update documentation first

---

## 11. Absolute Repository Rules

- No datasets in Git
- No training code in backend
- No AI logic in frontend
- No cross-layer imports
- No credentials or secrets in repository

Violations require immediate rollback and review.

---

## 12. Final Enforcement Rule

If a file does not clearly belong to one folder, it does not belong anywhere.

Stop and redesign before proceeding.

---

End of Document