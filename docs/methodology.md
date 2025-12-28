# Methodology

## 1. Purpose

This document describes the end to end methodology used to design, train,
integrate, and deploy the SPECTRA medical imaging software system.

The methodology ensures:
- Deterministic and reproducible AI behavior
- Clear separation between training and inference
- Offline capable desktop deployment
- Minimal user setup requirements
- Consistency between training and runtime preprocessing

---

## 2. System Overview

SPECTRA is a desktop application distributed as a single executable file.
It performs medical image analysis using multiple AI models.

The system operates in a fixed pipeline:

1. Input acquisition
2. Input normalization
3. Gatekeeper routing
4. Specialist model inference
5. Localization and highlighting
6. Unified report generation

All stages are deterministic and explicitly defined.

---

## 3. Input Methodology

### 3.1 Supported Inputs

The system accepts the following medical imaging formats:

- DICOM files (.dcm)
- NIfTI files (.nii, .nii.gz)

These inputs may represent:
- MRI scans
- CT scans
- X ray images

The user provides input through the desktop interface only.

No command line interaction is required.

---

### 3.2 Input Storage

All user provided files are stored temporarily on the local machine.

Reasons:
- Enable multi slice analysis
- Allow reprocessing without reupload
- Maintain consistency across models

No input data is uploaded to external servers by default.

---

## 4. Input Normalization

### 4.1 Conversion

All supported input formats are converted into intermediate PNG images.

Conversion rules:
- DICOM is decoded using medical imaging libraries
- NIfTI volumes are sliced along the axial axis
- Slice ordering is preserved
- Pixel intensity scaling is standardized

This step ensures compatibility with all downstream models.

---

### 4.2 Preprocessing Consistency

The preprocessing applied during inference is identical to the preprocessing
used during model training.

This includes:
- Image resizing
- Intensity normalization
- Channel formatting
- Data type conversion

No deviation is permitted between training and runtime pipelines.

---

## 5. Gatekeeper Model Methodology

### 5.1 Purpose

The Gatekeeper model performs anatomical routing.

It classifies input images into exactly one of:
- Brain
- Chest
- Bone

The Gatekeeper prevents incorrect specialist model usage.

---

### 5.2 Decision Logic

The Gatekeeper compares:
- User selected modality
- AI detected anatomical class

If they do not match:
- Inference is aborted
- A structured error message is returned
- No specialist model is executed

This prevents invalid predictions.

---

### 5.3 Training Method

The Gatekeeper is trained using:
- Heterogeneous datasets
- Multiple imaging sources
- Weakly related samples

The objective is anatomical recognition, not disease detection.

---

## 6. Specialist Model Methodology

### 6.1 Brain Tumor Model

- Operates on 3D volumes
- Uses multi channel MRI input
- Performs voxel level segmentation
- Outputs tumor regions

Analysis is performed across all slices and the full volume.

---

### 6.2 Chest Pneumonia Model

- Operates on X ray images
- Performs classification
- Uses probability based outputs
- Supports multiple views

Slice level and image level inference are combined.

---

### 6.3 Bone Fracture Model

- Operates on bone imaging
- Uses 2D or reduced 3D representations
- Detects fracture presence and location
- Supports multiple bone types

Middle slices are prioritized when volumetric data is present.

---

## 7. Multi Slice and Volume Analysis

For volumetric inputs:
- Every slice is analyzed
- Aggregation logic combines slice results
- Final decision reflects full scan context

This improves robustness and clinical relevance.

---

## 8. Localization and Highlighting

### 8.1 Region Detection

Specialist models produce:
- Segmentation masks
- Bounding boxes
- Activation maps

These are converted into screen overlays.

---

### 8.2 Visualization

Affected regions are highlighted using:
- Red outlines
- Transparent overlays
- Slice synchronized views

No post processing modifies the AI output geometry.

---

## 9. Output Methodology

### 9.1 Unified Report

All results are normalized into a single report structure.

The report contains:
- Model decision
- Confidence values
- Highlighted images
- Quantitative metrics

---

### 9.2 Export Formats

Outputs can be:
- Viewed directly in the application
- Exported as PDF
- Saved locally

All outputs are read only.

---

## 10. Application Architecture

### 10.1 Desktop Application

The system is packaged as a desktop executable.

Characteristics:
- No browser required
- No manual environment setup
- Local backend execution
- Optional internet access

---

### 10.2 Backend Execution

The backend:
- Loads models lazily
- Runs on localhost
- Is bundled with the executable
- Does not expose external ports

---

## 11. Training and Deployment Separation

Training:
- Performed in cloud environments
- Uses GPUs
- Produces model weight files

Deployment:
- Uses frozen inference engines
- No training code included
- No dataset bundled

This separation is strict.

---

## 12. Reproducibility Guarantees

The methodology enforces:
- Fixed preprocessing
- Locked model architectures
- Version controlled code
- Deterministic inference

Any change requires retraining and version bumping.

---

## 13. Limitations

- The software is for research and educational use
- It is not a certified medical device
- Clinical decisions must not rely solely on output

---

## 14. Summary

This methodology ensures that SPECTRA:
- Is robust and modular
- Is safe against incorrect usage
- Produces consistent results
- Is deployable at scale as a desktop application

All development must adhere strictly to this methodology.