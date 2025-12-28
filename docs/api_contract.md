# SPECTRA API Contract

Status: Authoritative  
Audience: Frontend, Backend, AI Developers  
Scope: Runtime inference API only

---

## 1. Purpose of This Document

This document defines the strict interface contract between the desktop frontend
(Electron + React) and the backend inference engine (FastAPI).

Its goals are to:
- Eliminate ambiguity in frontend-backend communication
- Guarantee stable integration across releases
- Define deterministic error handling
- Prevent undocumented API changes

Any modification to this contract requires coordinated changes in both frontend
and backend code.

---

## 2. Design Principles

The API follows these principles:

- Single public inference endpoint
- Stateless request handling
- JSON-only responses
- Explicit error reporting
- No frontend-side inference assumptions

The backend is the sole authority for:
- Input validation
- Modality routing
- Model execution
- Failure decisions

---

## 3. Base Configuration

The backend runs locally as part of the desktop application.

Configuration:

- Protocol: HTTP
- Host: 127.0.0.1
- Port: 8000
- Framework: FastAPI
- Authentication: None (local desktop application)

---

## 4. Endpoint Overview

The backend exposes exactly one public endpoint.

Endpoint:
- POST /analyze

This endpoint performs:
- Input validation
- Gatekeeper routing
- Specialist model inference
- Localization
- Report generation

---

## 5. Request Specification

### 5.1 Request Type

POST request with multipart form data.

Content-Type:
multipart/form-data

---

### 5.2 Form Fields

Required fields:

- file  
  Type: File  
  Description: Medical imaging input

Optional fields:

- modality_hint  
  Type: string  
  Allowed values: brain, chest, bone  
  Description: User-selected modality hint

The modality_hint is advisory only.
The Gatekeeper model always has final authority.

---

### 5.3 Accepted File Formats

The backend accepts only the following formats:

- DICOM files (.dcm)
- NIfTI files (.nii, .nii.gz)

All other formats must be rejected explicitly.

---

## 6. Backend Processing Flow

The backend must execute the following steps in order:

1. Temporarily store the uploaded file
2. Validate file format
3. Normalize input (DICOM or NIfTI parsing)
4. Convert to internal slice or volume representation
5. Apply deterministic preprocessing
6. Run Gatekeeper model
7. Evaluate Gatekeeper confidence
8. If ambiguous or mismatched, return error
9. Route to appropriate specialist model
10. Run multi-slice or volume inference
11. Aggregate predictions
12. Generate localization overlays
13. Generate clinical PDF report
14. Return structured JSON response

No step may be skipped or reordered.

---

## 7. Success Response

### 7.1 HTTP Status

200 OK

---

### 7.2 JSON Response Structure

The backend must return a JSON object with the following fields:

- status  
  Value: "success"

- routed_to  
  Value: brain, chest, or bone

- diagnosis  
  Value: Human-readable diagnosis string

- confidence  
  Value: Floating-point number between 0.0 and 1.0

- localization  
  Object containing localization artifacts

- report  
  Object containing report artifacts

---

### 7.3 Example Success Response

{
  "status": "success",
  "routed_to": "brain",
  "diagnosis": "Tumor detected",
  "confidence": 0.94,
  "localization": {
    "overlay_image": "overlay_001.png"
  },
  "report": {
    "pdf_path": "report_001.pdf"
  }
}

---

## 8. Error Responses

The backend must never return partial or malformed responses.
All errors must follow one of the schemas below.

---

### 8.1 Gatekeeper Mismatch Error

HTTP Status:
400 Bad Request

Response body:

{
  "status": "error",
  "error_type": "GATEKEEPER_MISMATCH",
  "detected": "chest",
  "message": "Uploaded scan does not match the selected modality."
}

---

### 8.2 Ambiguous Input Error

HTTP Status:
400 Bad Request

Response body:

{
  "status": "error",
  "error_type": "AMBIGUOUS_INPUT",
  "message": "Input could not be confidently routed to a specialist model."
}

---

### 8.3 Invalid File Format Error

HTTP Status:
400 Bad Request

Response body:

{
  "status": "error",
  "error_type": "INVALID_FORMAT",
  "message": "Unsupported file format."
}

---

### 8.4 Internal Inference Error

HTTP Status:
500 Internal Server Error

Response body:

{
  "status": "error",
  "error_type": "INFERENCE_FAILURE",
  "message": "Model execution failed."
}

---

## 9. Backend Guarantees

The backend guarantees the following:

- Always returns valid JSON
- Never crashes the frontend
- Never silently reroutes input
- Never returns partial inference results
- Never exposes stack traces or internal paths

---

## 10. Versioning Policy

- The API contract is versioned implicitly by repository release
- Breaking changes require:
  - Documentation update
  - Frontend update
  - Coordinated release

Backward-incompatible changes are not allowed mid-release.

---

## 11. Final Enforcement Rule

If frontend and backend interpretations of this contract differ,
the backend interpretation is authoritative.

Frontend logic must adapt to the backend, not the reverse.

---