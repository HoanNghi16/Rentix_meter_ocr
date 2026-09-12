##Project's Structure
rentix_meter_ocr/
│
├── dataset/
│   ├── raw/                 # ảnh gốc chụp từ công tơ
│   ├── processed/           # ảnh sau preprocessing
│   ├── train/
│   ├── val/
│   └── test/
│
├── labels/
│   ├── train.csv
│   ├── val.csv
│   └── test.csv
│
├── notebooks/
│   ├── 01_explore_dataset.ipynb
│   └── 02_test_ocr.ipynb
│
├── src/
│   ├── dataset/
│   │   ├── __init__.py
│   │   ├── loader.py
│   │   └── preprocessing.py
│   │
│   ├── models/
│   │   ├── __init__.py
│   │   └── ocr_model.py
│   │
│   ├── training/
│   │   ├── __init__.py
│   │   └── train.py
│   │
│   ├── evaluation/
│   │   ├── __init__.py
│   │   └── evaluate.py
│   │
│   └── inference/
│       ├── __init__.py
│       └── predict.py
│
├── checkpoints/
│   └── .gitkeep
│
├── outputs/
│   ├── predictions/
│   └── metrics/
│
├── requirements.txt
├── .gitignore
└── README.md