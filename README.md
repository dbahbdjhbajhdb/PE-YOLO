# PE-YOLO: A Parameter-Efficient YOLOv11-Based Detector for UAV Small Object Detection

Official implementation of **PE-YOLO**, a parameter-efficient YOLOv11-based detector for UAV small object detection.

This repository is based on Ultralytics YOLO and keeps the PE-YOLO model definitions, custom modules, and experiment utilities used for paper reproduction.

## Main Files

- `ultralytics/cfg/models/11/PE-YOLO.yaml`: PE-YOLO model configuration.
- `ultralytics/cfg/models/11/PE-YOLO(n).yaml`: nano-scale PE-YOLO configuration.
- `ultralytics/cfg/models/11/PE-YOLO(l).yaml`: large-scale PE-YOLO configuration.
- `ultralytics/cfg/datasets/VisDrone.yaml`: VisDrone dataset configuration.
- `ultralytics/nn/modules/block.py`: custom backbone and feature modules.
- `ultralytics/nn/modules/head.py`: custom detection heads.
- `ultralytics/nn/tasks.py`: model parsing and module registration.
- `tools/`: evaluation, conversion, and visualization scripts.
- `assets/`: paper figures and comparison plots.

## Installation

Create a clean environment:

```bash
conda env create -f environment.yml
conda activate pe-yolo
```

Or create the environment manually and install the repository in editable mode:

```bash
conda create -n pe-yolo python=3.10 -y
conda activate pe-yolo
pip install -e .
```

This follows the standard Ultralytics editable-install workflow. If PyTorch is not installed automatically for your CUDA version, install the matching PyTorch build first, then run `pip install -e .`.

## Dataset

The experiments use VisDrone2019-DET. The dataset configuration is provided in:

```text
ultralytics/cfg/datasets/VisDrone.yaml
```

Expected dataset root:

```text
datasets/
`-- VisDrone/
    |-- images/
    |   |-- train/
    |   |-- val/
    |   `-- test/
    `-- labels/
        |-- train/
        |-- val/
        `-- test/
```

The raw dataset and generated training outputs are intentionally excluded from Git.

## Training

Train PE-YOLO on VisDrone:

```bash
yolo detect train model=ultralytics/cfg/models/11/PE-YOLO.yaml data=VisDrone.yaml epochs=200 imgsz=640 batch=16
```

Nano and large variants:

```bash
yolo detect train model=ultralytics/cfg/models/11/PE-YOLO(n).yaml data=VisDrone.yaml epochs=200 imgsz=640 batch=16
yolo detect train model=ultralytics/cfg/models/11/PE-YOLO(l).yaml data=VisDrone.yaml epochs=200 imgsz=640 batch=16
```

## Evaluation

Validate a trained checkpoint:

```bash
yolo detect val model=runs/detect/train/weights/best.pt data=VisDrone.yaml imgsz=640 save_json=True
```

Optional utilities are kept in `tools/`, including YOLO-to-COCO conversion, AP calculation, heatmap visualization, and layer/module inspection scripts.

## Repository Cleanup

This release intentionally removes the original Ultralytics documentation site, example gallery, Docker files, GitHub Actions, Dependabot configuration, and temporary experiment outputs. The core Ultralytics package code is retained to keep PE-YOLO compatible with the original training and validation pipeline.

## Notes

- Model weights, datasets, logs, and generated prediction files are not committed to this repository.
- The original Ultralytics README is preserved as `README.ultralytics.md`.
- Please cite the paper when using this code after the paper is published.
