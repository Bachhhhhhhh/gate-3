# Gate 3 Deliverable: Demo Video Draft

Target length: 3-5 minutes.

Required format: slides pitch + live demo.

Demo video evidence folder:

```text
https://drive.google.com/drive/folders/1tjFNgdLY_eRJ8PTQr1caMf6FQ1IobFcj?usp=sharing
```

## Video Structure

### 0:00-0:30 - Problem

Show or explain:

- Manual surface inspection is slow and inconsistent.
- Inspectors need visual evidence, report history, and user-specific storage.
- Target defects: cracks, peeling/spalling, exposed substrate, and surface
  anomalies.

Suggested narration:

```text
Inspectra helps inspectors capture surface images, run AI defect screening, and
store reports by user account. The MVP focuses on construction/surface defect
inspection such as cracks and peeling paint or plaster.
```

### 0:30-1:15 - Product Overview

Show:

- Deployed web app URL.
- Login page.
- Dashboard.
- Report list.

URL:

```text
https://c2-app-089-production.up.railway.app
```

Demo login:

```text
inspector@inspectra.ai / inspect123
```

### 1:15-2:20 - Live Demo: Web Upload to VLM Report

Steps:

1. Click `Tao luot kiem tra`.
2. Upload a local surface image.
3. Run VLM inspection.
4. Wait for report creation.
5. Open report detail.
6. Show:
   - source image,
   - segmentation/overlay image,
   - defect count,
   - confidence,
   - result JSON artifact,
   - reviewer note.

Expected evidence from production smoke test:

```text
Report ID: VLM-260628-3594
Status: fail
Defect count: 2
Artifacts: result_json, source_image, segmentation_overlay, overlay
```

### 2:20-3:00 - Live Demo: Report Confirmation Flow

Show:

1. Open a report detail page.
2. Click `Xac nhan ket qua`.
3. Show visible confirmation state.
4. Return to report list.
5. Show the approval flag indicating whether a report has been confirmed.

Purpose:

- Demonstrates the human-in-the-loop guardrail.
- Makes clear that AI output is reviewed, not blindly accepted.

### 3:00-3:40 - Robot / ESP32-CAM Path

Show available robot/camera flow:

```powershell
cd "<repo>\surface_inspection_product\ros_robot_control"
$env:PYTHONPATH = ".\ros_ws\src\surface_inspection_robot"

python -m surface_inspection_robot.cli esp32-inspect `
  --camera-url http://<ESP32_CAMERA_IP> `
  --capture-dir .\captures `
  --runs-dir ..\vlm_surface_inspection\runs `
  --provider novita `
  --detail low `
  --grid 2 `
  --timeout 30 `
  --website-url https://inspectra-api-production.up.railway.app `
  --website-email inspector@inspectra.ai `
  --website-password inspect123 `
  --product "ESP32-CAM wall demo" `
  --robot "ESP32-CAM"
```

Important wording:

- The camera is used for inspection capture.
- Camera frames are not used for route planning.
- Robot movement footage is a hardware movement demo if it is manually
  controlled.

### 3:40-4:30 - Evaluation and Guardrails

Show metrics slide:

```text
Dataset: SDNET2018 subset, 168 images
Model: qwen/qwen3-vl-235b-a22b-instruct
Accuracy: 70.83%
Precision: 100.00%
Recall: 41.67%
F1: 58.82%
Average latency: 6.47 sec/image
Cost: $0.1148 total
```

Explain:

- Precision is strong in the subset.
- Recall still needs improvement.
- Human review remains required.

### 4:30-5:00 - Next Steps

Mention:

- Improve recall on real images.
- Add more defect classes and better segmentation.
- Move SQLite demo data to PostgreSQL.
- Store artifacts in S3-compatible storage.
- Add background jobs for long VLM processing.
- Continue robot integration through CAD-guided deterministic planning.

## Demo Checklist

- Web app opens.
- Login works.
- Upload image works.
- Report detail shows overlay/segmentation artifact.
- Report appears in report list.
- Confirmation flag appears after report approval.
- Evaluation metrics slide is visible.
- Guardrails are stated clearly.
