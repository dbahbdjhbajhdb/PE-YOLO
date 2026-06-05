# PE-YOLO

Official implementation of **PE-YOLO** for object detection in drone imagery.

This repository is based on Ultralytics YOLO and contains the model definitions, custom modules, and experiment scripts used for the paper experiments.

## Main Files

- `ultralytics/cfg/models/11/PE-YOLO.yaml`: PE-YOLO model configuration.
- `ultralytics/cfg/models/11/PE-YOLO(n).yaml`: nano-scale PE-YOLO configuration.
- `ultralytics/cfg/models/11/PE-YOLO(l).yaml`: large-scale PE-YOLO configuration.
- `ultralytics/cfg/datasets/VisDrone.yaml`: VisDrone dataset configuration.
- `ultralytics/nn/modules/block.py`: custom convolution modules.
- `ultralytics/nn/modules/head.py`: custom detection head.
- `ultralytics/nn/tasks.py`: model parsing and module registration.

## Installation

```bash
conda create -n pe-yolo python=3.10 -y
conda activate pe-yolo
pip install -e .
```

If PyTorch is not installed automatically for your CUDA version, install it first from the official PyTorch instructions, then run `pip install -e .`.

## Dataset

The experiments use VisDrone2019-DET. The dataset configuration is provided in:

```text
ultralytics/cfg/datasets/VisDrone.yaml
```

Expected dataset root:

```text
datasets/
└── VisDrone/
    ├── images/
    │   ├── train/
    │   ├── val/
    │   └── test/
    └── labels/
        ├── train/
        ├── val/
        └── test/
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

## Notes

- Model weights, datasets, logs, and generated prediction files are not committed to this repository.
- The original Ultralytics README is preserved as `README.ultralytics.md`.
- Please cite the paper when using this code after the paper is published.
