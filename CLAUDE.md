
@'

# SadakYatri - Claude Code Instructions

## Project

Vision-based road damage detection for Nepal. Pipeline:
video -> YOLOv11 detection -> tracking -> severity -> GPS -> Django API
-> geo-dedup -> priority -> maintenance agent.
Scope is V1-V6 plus a thin V7 agent. Do not build V8-V10.

## Environment

- Windows, PowerShell. Never suggest Ctrl+C as a copy shortcut.
- Activate first: .venv\Scripts\activate
- torch is the CUDA build installed from the PyTorch index URL BEFORE
  ultralytics. If reinstalling, keep that order or training silently
  falls back to CPU.
- Verify GPU before any training run:
  python -c "import torch; print(torch.cuda.is_available())"

## Hardware limits

RTX 2050, 4GB VRAM. Default training config:
--model yolo11s.pt --imgsz 640 --batch 8 --epochs 100
If OOM, reduce batch before imgsz - small potholes need resolution.
Never propose a config needing more VRAM without flagging the cost.

## Non-negotiable rules

- Dataset splits are by RECORDING, never random by frame. Frames from one
  video are near-duplicates; random splitting leaks and inflates mAP.
  Never write a script that splits at frame level.
- Priority is computed by calculate_priority(), never by an LLM.
- Config through constructors. No hardcoded paths, no globals.

## Code style

- One class per pipeline stage, one public method.
- Depend on abstract base classes, not concrete siblings.
- Public API minimal; internals prefixed with underscore.
- After significant code, add a short "Why this way" note naming the design
  principle and what breaks with the naive approach.

## Working style

- Correct me before coding. If a request is wrong, inefficient or out of
  scope, say so, explain, propose the alternative, then wait.
- Build incrementally. Do not dump finished subsystems.
- Ask before installing new dependencies.

## Commands

- Train:    yolo detect train data=data.yaml model=yolo11s.pt imgsz=640 batch=8 epochs=100
- Validate: yolo detect val model=runs/detect/train/weights/best.pt
- Backend:  python manage.py runserver
  '@ | Set-Content -Path CLAUDE.md -Encoding UTF8
