## ENVIRONMENT SETUP — S.P.E.C.T.R.A.
## PURPOSE:
## - Deterministic development
## - Reproducible AI training
## - Stable backend inference
## - Reliable desktop packaging
---

environment_setup:

  ---
  ## PROJECT METADATA
  ---
  project_name: SPECTRA

  purpose:
    - deterministic_development
    - reproducible_training
    - stable_backend_inference
    - reliable_desktop_packaging

  ---
  ## SUPPORTED OPERATING SYSTEMS
  ---
  supported_operating_systems:

    development:
      - windows_10_64bit
      - windows_11_64bit
      - ubuntu_20_04_64bit
      - ubuntu_22_04_64bit

    end_user:
      - windows_10_64bit
      - windows_11_64bit

  ---
  ## GLOBAL TOOLCHAIN REQUIREMENTS
  ---
  global_toolchain:

    python:
      min_version: "3.9"
      max_version: "3.11"

    nodejs:
      min_version: "18_lts"

    npm:
      source: bundled_with_node

    git:
      version: latest_stable

    verification_commands:
      - python --version
      - node --version
      - npm --version
      - git --version

  ---
  ## BACKEND RUNTIME ENVIRONMENT (FASTAPI + INFERENCE)
  ---
  backend_environment:

    purpose:
      - fastapi_runtime
      - ai_model_inference
      - medical_image_preprocessing
      - pdf_report_generation

    virtual_environment:
      location: backend/venv_backend
      creation_command: python -m venv venv_backend
      global_python_allowed: false

    dependencies:
      fastapi: "0.110.0"
      uvicorn: "0.29.0"
      torch: "2.2.0"
      monai: "1.3.0"
      numpy: "1.26.4"
      opencv_python: "4.9.0.80"
      pydicom: "2.4.4"
      nibabel: "5.2.1"
      pillow: "10.2.0"
      reportlab: "4.1.0"

    installation_command: pip install -r requirements.txt

    runtime_constraints:
      cuda_required: false
      training_allowed: false

    validation_imports:
      - fastapi
      - torch
      - monai
      - cv2
      - pydicom
      - nibabel
      - reportlab

  ---
  ## FRONTEND DEVELOPMENT ENVIRONMENT (ELECTRON + REACT)
  ---
  frontend_environment:

    purpose:
      - electron_desktop_shell
      - react_user_interface
      - backend_process_launcher

    stack:
      ui_framework: react
      desktop_runtime: electron
      build_tool: vite
      http_client: axios
      packager: electron_builder

    installation_commands:
      - npm install
      - npm install electron electron-builder axios

    validation_command: npm run dev

  ---
  ## TRAINING ENVIRONMENT (AI MODEL DEVELOPMENT)
  ---
  training_environment:

    platform: google_colab
    gpu_required: true
    local_training_allowed: false

    notebooks:
      gatekeeper: gatekeeper_train.ipynb
      brain: brain_train.ipynb
      chest: chest_train.ipynb
      bone: bone_train.ipynb

    dependencies_install_command:
      - pip install torch torchvision monai numpy opencv-python pydicom nibabel

    dataset_mount:
      provider: google_drive
      example_path: /content/drive/MyDrive/DATASETS/SPECTRA
      copy_into_repo: false

  ---
  ## DATA STORAGE RULES
  ---
  data_storage_rules:

    repository:
      raw_data_committed: false
      ignored_paths:
        - data/raw

    external_storage:
      allowed:
        - local_disk
        - external_drive
        - cloud_storage
      documentation_required: true

  ---
  ## MODEL ARTIFACT HANDLING
  ---
  model_artifacts:

    training_output_formats:
      - .pt
      - .pth

    git_commit_allowed: false

    runtime_inclusion:
      location: models/
      included_during_packaging: true

  ---
  ## PACKAGING CONSTRAINTS
  ---
  packaging_constraints:

    backend_environment_only: true
    training_libraries_allowed: false
    unused_dependencies_allowed: false
    path_policy: relative_only

  ---
  ## COMPLIANCE & GOVERNANCE
  ---
  compliance:

    deviation_allowed: false

    consequences:
      - non_reproducible_results
      - runtime_failures
      - packaging_errors
      - invalid_experiments

  change_control:
    approval_required: true
    documentation_update_required: true
    commit_message_required: true