# SPECTRA Architecture Rules

Status: Authoritative  
Audience: All developers  
Scope: Entire system architecture

---

## 1. Purpose of This Document

This document defines the mandatory architectural rules for the SPECTRA software system.

Its objectives are to:
- Enforce clean separation of concerns
- Prevent cross-layer coupling
- Ensure deterministic behavior
- Enable reliable packaging into a single executable
- Maintain long-term maintainability and extensibility

Any violation of these rules is considered a blocking architectural defect.

---

## 2. Architectural Philosophy

SPECTRA follows a strictly layered architecture.

Each layer has:
- A single, well-defined responsibility
- Explicitly allowed dependencies
- Explicitly forbidden dependencies

No layer may assume internal behavior of another layer.
Communication between layers must occur only through defined contracts.

If a piece of code does not clearly belong to one layer, it must not exist.

---

## 3. System Layers Overview

The system is composed of the following layers:

- Frontend layer
- Backend layer
- Model artifact layer
- Data layer
- Training and experimentation layer
- Documentation layer

Each layer is enforced by directory boundaries.

---

## 4. Frontend Layer

Directory:
frontend/

### Responsibility
The frontend layer is responsible only for user interaction and visualization.

It handles:
- File selection and upload
- Display of results
- Display of error messages
- Download of generated reports

### Allowed Dependencies
- Electron
- React
- JavaScript or TypeScript
- HTML and CSS
- HTTP client libraries

### Forbidden Dependencies
- AI or machine learning libraries
- Python code
- Medical preprocessing logic
- Dataset access
- Model loading or inference
- Training logic

Rule:
The frontend must remain completely unaware of model internals and medical logic.

---

## 5. Backend Layer

Directory:
backend/

### Responsibility
The backend layer is responsible for all computation and orchestration.

It handles:
- Input validation
- Input normalization
- Model routing
- Inference execution
- Post-processing
- Report generation

### Allowed Dependencies
- Python
- FastAPI
- PyTorch (inference only)
- MONAI (inference only)
- Numerical and image processing libraries

### Forbidden Dependencies
- Training code
- Dataset download logic
- Randomized preprocessing at runtime
- Frontend rendering logic
- Notebook execution

Rule:
The backend must never train models or depend on training utilities.

---

## 6. Model Artifact Layer

Directory:
models/

### Responsibility
The model artifact layer stores trained, frozen model weights used at runtime.

### Allowed Contents
- gatekeeper.pt
- brain.pt
- chest.pt
- bone.pt

### Rules
- Models must be trained externally
- Models must be immutable during runtime
- Models must be loaded in inference mode only

### Forbidden Contents
- Raw datasets
- Training scripts
- Optimizer states
- Intermediate checkpoints

---

## 7. Data Layer

Directory:
data/

### 7.1 Raw Data

Directory:
data/raw/

Purpose:
Temporary local storage of original datasets.

Rules:
- Must never be committed to version control
- Must never be accessed by runtime backend
- Exists only on local machines or external storage

---

### 7.2 Processed Data

Directory:
data/processed/

Purpose:
Deterministic preprocessing outputs used for training and validation.

Rules:
- Must exactly match inference preprocessing logic
- May contain small representative samples
- Large datasets must remain external

---

## 8. Training and Experimentation Layer

Directories:
- notebooks/
- scripts/

### Responsibility
This layer exists only for training, experimentation, and validation.

It includes:
- Google Colab notebooks
- Training scripts
- Dataset analysis tools

Rules:
- Must never be imported by backend runtime code
- Must never be required for application execution
- Changes here must not affect runtime behavior

---

## 9. Reporting Layer

Directory:
reports/

### Responsibility
Stores generated output artifacts.

Includes:
- PDF reports
- Annotated images
- Temporary inference outputs

Rules:
- Generated at runtime only
- Safe to delete and regenerate
- Must not contain source code or logic

---

## 10. Documentation Layer

Directory:
docs/

### Responsibility
Defines system contracts and methodology.

Required documents include:
- architecture_rules.md
- api_contract.md
- folder_responsibility.md

Rules:
- Documentation must be kept consistent with implementation
- Architectural changes require documentation updates first

---

## 11. Determinism and Reproducibility

The system must satisfy the following:

- Inference must be deterministic
- Preprocessing must be identical between training and inference
- No random behavior is allowed at runtime
- Any randomness during training must be seed-controlled

---

## 12. Packaging Rules

- The final deliverable must be a single executable file
- End users must not install Python, Node.js, or CUDA
- Backend services must be bundled within the desktop application
- All runtime dependencies must be packaged explicitly

---

## 13. Enforcement Rules

- No datasets in version control
- No training code in backend
- No AI logic in frontend
- No cross-layer imports
- No secrets or credentials in the repository

Violations require immediate rollback and architectural review.

---

## 14. Final Enforcement Statement

If a file, module, or dependency does not clearly belong to a single layer,
it must be removed or redesigned.

Architecture clarity takes precedence over convenience.

---

End of Document
