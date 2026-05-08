Based on the thesis documentation, here is a **README-style** summary of the project:

---

# Ge'ez Script Recognition Using CRNN with Preprocessing

## Overview

This project provides an automated OCR system for recognizing **Ge'ez script** (Ethiopic) in historical manuscripts and printed documents. The system uses a hybrid **CRNN (Convolutional Recurrent Neural Network)** with CTC loss, achieving 99.42% character accuracy on isolated text lines.

## Key Features

- **317-character syllabary** support (plus CTC blank token)
- **Preprocessing pipeline** for historical document degradation:
  - Border removal
  - Skew correction (>0.5° threshold)
  - Bilateral filtering (noise removal)
  - CLAHE contrast enhancement
  - Adaptive thresholding (binarization)
- **CRNN architecture**: 5 CNN blocks + 2-layer BiLSTM (8.87M parameters)
- **Data augmentation**: 3× multiplier (rotation, translation, scale, Gaussian noise)
- **Full-page extraction**: Contour-based line detection + tiled CNN features (avoids squishing wide lines)

## Results

| Metric | Value |
|--------|-------|
| Test CER (line-level) | 0.58% |
| Character accuracy | 99.42% |
| Page-level CER (sample) | 26.35% |
| Training samples/epoch | 121,569 (after 3× augmentation) |
| Best validation CER | 0.47% (epoch 5) |

## Dataset

- **Raw pairs**: 102,113 PNG + GT text files
- **After cleaning**: 57,891 pairs (dedup, empty GT removal, corrupted images)
- **Split**: 70/15/15 by text group (no leakage)
- **Character set**: 317 classes

## Requirements

- Python 3.x
- PyTorch (CUDA)
- OpenCV
- Pillow
- Albumentations
- NumPy

## Project Structure

```
├── preprocessing.py      # Border remove, deskew, denoise, CLAHE, binarize
├── model.py              # CRNN definition (CNN + BiLSTM + CTC)
├── dataset.py            # GeezDataset with augmentation
├── train.py              # Training loop (Adam, ReduceLROnPlateau)
├── evaluate.py           # CER, WER, error analysis
├── page_extractor.py     # FullPageExtractor (line detection + tiled CNN)
└── char_mappings.json    # 317-character vocabulary + CTC blank
```

## Key Design Decisions

1. **No squishing during inference** – Natural width preserved; tiled CNN avoids 24% CER penalty
2. **Group-based split** – Same text strings never appear across train/val/test
3. **Bilateral filter** – Preserves character edges while removing noise
4. **Asymmetric pooling** – (2,1) strides collapse height while preserving width sequence

## Limitations

- Single-column text only (no marginal notes or complex layouts)
- No language model post-processing
- Page-level CER still high (26.35%) – main area for future work

## Citation (Thesis)

Leake Adisalem, Leake Abrha, Frewenyi Asgedom, Biniyam Gebreyesus. *"Ge'ez Script Recognition Using Convolutional Recurrent Neural Network with Preprocessing for Historical Manuscript Digitization."* Mekelle Institute of Technology, 2026.

---

Want me to adjust the format (e.g., add installation steps, commands, or a "Quick Start" section)?
