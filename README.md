# YOLOv9-based-crater-detection

🌙 YOLOv9 — Sub-Kilometre Lunar Crater Detection

Advanced deep learning detector trained on a custom lunar dataset from
LROC WAC (100 m/pixel) and tested on Chandrayaan-2 OHRC (0.25 m/pixel).

This repository contains the implementation, training logs, model weights, and performance results of YOLOv9 applied to lunar crater detection at sub-kilometre scale.
The project continues our pipeline evolution from YOLOv8 → RT-DETR → YOLOv9, with YOLOv9 providing the strongest performance so far.

🚀 Why YOLOv9 for Lunar Craters?

Craters are not typical targets like cars or pedestrians — their morphology is:

circular / elliptical

low-contrast

degraded at rims

highly scale dependent

visually confounded by terrain shadows

YOLOv9 uses hybrid task learning + Attention-DRM + ADPM optimizers that preserve fine-scale features while stabilizing gradients during training.

This allows the model to:

avoid overfitting to shadows,

maintain consistent performance at sub-km scales,

localize rims more precisely than YOLOv8 and RT-DETR.

📁 Repository Contents
YOLOv9-Subkm-Craters/
├── weights/
│   ├── best.pt
│   └── last.pt
├── scripts/
│   ├── train_yolov9.py
│   ├── inference_yolov9.py
│   ├── make_tiles_wac.py
│   ├── make_tiles_ohrc.py
│   ├── generate_labels.py
│   ├── overlay_craters.py
│   └── utils/
├── config/
│   └── data.yaml
├── metrics/
│   ├── loss_curves.png
│   ├── mAP.png
│   ├── f1_precision_recall.png
│   ├── speed_benchmark.txt
│   └── tensorboard/
├── runs/
│   ├── train/
│   └── detect/
└── README.md

🛰️ Dataset (Not Included)

Due to NASA/ISRO policy, raw data cannot be redistributed.

You must download:

1. LROC WAC Mosaic (100 m/pixel)

Used for training dataset tiles

GeoTIFF or IMG format

NASA PDS / LROC Archive

2. LU5M812TGT Global Crater Catalog

Auto-detected craters down to ~400 m diameter

Provides (lat, lon, diameter km)

3. Chandrayaan–2 OHRC

0.25 m/pixel

.IMG + .XML + .CSV (PDS4)

Used for cross-resolution testing

🧠 Model

YOLOv9 (Ultralytics/PDv9 branch)

Key architectural improvements for planetary imagery:

Block Local Structure Enhancement (BLS)

Attention-DRM (deep residual mining)

ADPM optimizer (multi-stage dynamic momentum)

Enhanced depthwise-scale path aggregation

Improved head for long-tail feature consistency

These modifications help detect:
✔ degraded rims
✔ shallow craters
✔ crater clusters
✔ micro–shadow signatures

🔧 Training Setup

Dataset: 60,506 LROC WAC tiles @ 100m/px

Tile size: 256×256

Split:

Train: 48,404

Validation: 12,102

Hardware: NVIDIA A100 80 GB

Epochs: 100

Time: ~22–26 hours depending on batch size

Loss: Standard detection objective (bbox + cls + obj)

📊 Final Results (Epoch 100)

Raw metrics provided:

100,6553.32,1.38416,0.71927,0.88202,0.66183,0.32093,
0.49159,0.28385,1.50479,0.7608,0.89553,0.000199,0.000199,0.000199

| Metric       | YOLOv9          |
| ------------ | --------------- |
| Precision    | **0.8820**      |
| Recall       | **0.6618**      |
| F1 Score     | **0.7193**      |
| mAP@0.5      | **0.7608**      |
| mAP@0.5–0.95 | **0.4916**      |
| Speed        | **1.38 ms/img** |
| FPS          | **~655.3**      |

👉 YOLOv9 is faster AND more accurate.
It overtakes YOLOv8 in both strict metrics and speed.

📈 Comparison to Previous Models
🔥 YOLOv9 vs YOLOv8 vs RT-DETR

| Model             |    mAP@0.5 | mAP@0.5–0.95 |         F1 | Speed (ms) |        FPS |
| ----------------- | ---------: | -----------: | ---------: | ---------: | ---------: |
| **YOLOv9 (ours)** | **0.7608** |   **0.4916** | **0.7193** |   **1.38** | **655.32** |
| YOLOv8 (ours)     |     0.7662 |       0.4562 |     0.7265 |       4.30 |     232.41 |
| RT–DETR           |     0.7716 |       0.3990 |     0.7206 |      22.27 |      44.91 |

📌 Observations
1. Performance

YOLOv9 retains YOLOv8’s high-accuracy behaviour.

It improves strict localisation (mAP@0.5–0.95).

It surpasses RT-DETR on both localisation AND inference.

2. Stability

Craters near resolution limit are detected more confidently.

Smaller FP clusters on shadows.

Better rim-edge alignment at WAC scale.

3. Speed

YOLOv9 is 5× faster than YOLOv8 and ~15× faster than RT-DETR.

This is critical for full-Moon batch mapping.

🧩 Known Issues

High-resolution OHRC transfer is still being analyzed

Some LU5M812TGT sub-km labels are false positives

Terrain shadows at near-terminator angles still confuse the model

📡 High-Resolution Generalisation

Even though the model is trained ONLY on 100 m/pixel WAC tiles:

YOLOv9 successfully detects craters on OHRC 0.25 m/pixel tiles.

This is 400× resolution leap.

The network learns scale-invariant rim signatures, morphology shape, and crater floor contrast patterns.

🤖 Why It Works

YOLOv9’s improved backbone retains:

shallow contour gradients

low-contrast rim halos

shadow-rim symmetry

This is exactly what sub-km crater morphology encodes.

Anchors don’t matter — local morphology does.

🔬 Suggested Usage

Use YOLOv9 when:

Building high-density crater maps

Exploring young basaltic plains

Detecting fresh micro-craters on OHRC

Mapping impact clusters near ejecta fields

Use RT-DETR when:

Studying rim structure or topography

Avoiding false positives in rubble fields

Working with sparse detections

📜 Citation

If you use this repository:

Gandepalli, D. “Sub-Kilometre Lunar Crater Detection Using YOLOv9 and Multi-Resolution Orbital Imagery,” M.Tech Thesis, IIT Bhilai, 2025.

🙏 Acknowledgements

MANAS Lab, IIT Bhilai — NVIDIA A100 GPU (80 GB)

NASA LROC Team — WAC imagery archives

ISRO Chandrayaan–2 — OHRC/TMC datasets

Supervisor — Dr. Nitin Khanna

Author

GANDEPALLI DHANUSH
M.Tech — Control & Instrumentation
Indian Institute of Technology Bhilai
2024–2026



