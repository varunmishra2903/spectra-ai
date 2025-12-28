# 🔌 SPECTRA — Backend API Contract

> **Status:** Authoritative  
> **Audience:** Backend, Frontend, AI Developers  
> **Owner:** Backend Architecture  
> **Applies To:** Runtime API only (not training)

---

## 📌 Purpose of This Document

This document defines the **strict contract** between:

- Frontend (Electron + React)
- Backend (FastAPI inference engine)

It guarantees:
- Stable integration
- Predictable responses
- Zero ambiguity in error handling

⚠️ **Any change to this contract requires coordinated updates across backend and frontend.**

---

## 🧱 API Design Principles

- Single public inference endpoint
- JSON-only communication
- Backend is stateless per request
- Frontend never infers logic from UI state

---

## 🌐 Base Configuration

| Parameter | Value |
|--------|------|
| Protocol | HTTP |
| Host | `127.0.0.1` |
| Port | `8000` |
| Framework | FastAPI |
| Auth | None (local app) |

---

## 🚀 Endpoint Overview

### `POST /analyze`

**Purpose**
- Accepts medical imaging input
- Runs Gatekeeper + Specialist model
- Returns diagnosis, localization, and report artifacts

---

## 📥 Request Specification

### Request Type
```http
POST /analyze
Content-Type: multipart/form-data
