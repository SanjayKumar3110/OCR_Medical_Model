
# OCR Module – MediScan

## Overview

This module handles **Optical Character Recognition (OCR)** for the **MediScan – AI Prescription Analyzer** project.
It extracts textual information (medicine names, dosage, patient info, etc.) from handwritten or printed medical prescriptions.

The OCR module forms the **first phase** of the MediScan pipeline and provides structured text output for downstream components like **NER (Entity Recognition)** and **Disease Prediction**.

---

## Features

* Preprocessing of prescription images (grayscale, denoising, thresholding)
* Text extraction using **PaddleOCR** or **Tesseract**
* Confidence-based word filtering
* Optional comparison across OCR engines for improved accuracy
* Designed to handle both printed and handwritten text

---

## Folder Structure

```
modules/
  └── ocr/
      │── ocr_notebook.ipynb        # Jupyter Notebook for training & testing OCR
      │── sample_images/             # Example prescription images
      │── output/                    # Extracted text results or JSON files
      └── README.md
```

---

## How It Works

1. **Input:** Prescription image (`.jpg`, `.png`)
2. **Image Preprocessing:**

   * Noise removal
   * Grayscale conversion
   * Contrast and threshold adjustment
3. **Text Extraction:**

   * Runs OCR using PaddleOCR (recommended) or Tesseract
   * Returns detected words and their confidence scores
4. **Postprocessing:**

   * Filters out low-confidence words
   * Outputs structured text for NER model

---

## Installation

1. Navigate to OCR module:

   ```bash
   cd modules/ocr
   ```

2. Install dependencies:

   ```bash
   pip install paddleocr opencv-python matplotlib
   ```

3. (Optional) For Tesseract:

   ```bash
   pip install pytesseract
   ```

---

## Usage

### Run the Notebook

Open `ocr_notebook.ipynb` and run all cells sequentially.

---

## Output Example

**Input:**

Image path is act as input

![sample prescription](sample_images/prescription1.jpg)

**Output:**

```
Paracetamol 500mg  |  Confidence: 0.92
Take 1 tablet twice daily
Dr. Kumar Clinic
```

---

## Integration with MediScan

The OCR output feeds into:

```
OCR → NER (BioBERT) → RAG Knowledge Retrieval → Doctor Finder → Final Report
```

This ensures all downstream components work with clean, structured text extracted from raw images.

---

## Future Work

* Improve recognition accuracy for handwritten prescriptions
* Add multilingual support (Tamil, Hindi, etc.)
* Integrate adaptive thresholding and vision transformer-based OCR models

---

