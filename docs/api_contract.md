# 🔌 SPECTRA — Backend API Contract

> **Status:** Locked & Authoritative  
> **Applies To:** Frontend ↔ Backend communication  
> **Owner:** Backend Architecture  
> **Change Policy:** Breaking changes require version bump

---

## 📌 Purpose

This document defines the **exact API interface** between the **Electron frontend** and the **FastAPI backend**.

The frontend must treat the backend as a **black box** and strictly follow this contract.

---

## 🌐 Base Configuration

- **Protocol:** HTTP
- **Host:** `127.0.0.1`
- **Port:** `8000`
- **Base URL:**  
