# MediScan – AI Prescription Analyzer

## Overview

MediScan is an AI-powered system designed to automatically analyze medical prescriptions (images or text), extract medicine information, and provide insights including:

- Predicted diseases related to prescribed medicines  
- Associated symptoms  
- Possible side effects  
- Preventive health tips  

The project uses a **Hybrid RAG (Retrieval-Augmented Generation)** approach, combining a fine-tuned **Named Entity Recognition (NER)** model with a **retrieval-based medical knowledge base** for accurate and updatable predictions.

---

## Features

- **OCR Module**: Extracts text from prescription images using Tesseract or PaddleOCR.  
- **NER Model**: Fine-tuned BioBERT model identifies medicines, symptoms, and dosages.  
- **RAG System**: Retrieves disease names, side effects, and health tips from a structured medical database.  
- **End-to-End Pipeline**: Handles both image and text input.  
- **API & UI**: Provides REST API and optional web interface for uploads and results visualization.  

---

## Project Structure

```
MediScan/
│
│── data/
│    ├── prescriptions/               # Prescription images or text
│    ├── annotated_ner_data.json      # NER training annotations
│    ├── drug_disease_mapping.csv     # Medicine → Disease
│    ├── drug_side_effects.csv        # Medicine → Side effects
│    ├── health_tips.csv              # Disease → Preventive tips
│    └── vector_store/                # FAISS or ElasticSearch index
│
│── models/
│    ├── ocr_model/                   # OCR engine (Tesseract/PaddleOCR)
│    └── ner_model/                   # Fine-tuned BioBERT for medicine extraction
│
│── src/
│    ├── ocr_extraction.py            # OCR text extraction module
│    ├── preprocess.py                # Text cleaning and formatting
│    ├── train_ner.py                 # Script to fine-tune NER model
│    ├── build_knowledge_db.py        # Builds CSV-based knowledge base
│    ├── retriever.py                 # Retrieval using FAISS/ElasticSearch
│    ├── rag_pipeline.py              # End-to-end inference pipeline
│    └── utils.py                     # Shared helper functions
│
│── app/
│    ├── main.py                      # FastAPI or Flask backend
│    └── ui/                          # Streamlit/React UI for uploads
│
│── requirements.txt
└── README.md
```

---

## RAG Pipeline Overview

1. **OCR**: Converts uploaded prescription image into text.  
2. **NER**: Extracts medicines, symptoms, and dosages.  
3. **Retriever**: Searches the medical knowledge base for:  
   - Disease related to medicines  
   - Side effects of each medicine  
   - Preventive health tips for the disease  
4. **Output**: Returns structured JSON or displays it in UI.  

---

## Installation

### 1. Clone Repository
```bash
git clone https://github.com/your-username/MediScan.git
cd MediScan
```

### 2. Setup Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # For Linux/Mac
venv\Scripts\activate     # For Windows
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

---

## Training

### Fine-Tune NER Model
Prepare annotated data in `data/annotated_ner_data.json` and run:
```bash
python src/train_ner.py
```
Model will be saved to `models/ner_model/`.

### Build Knowledge Base
Populate:
- `drug_disease_mapping.csv`
- `drug_side_effects.csv`
- `health_tips.csv`

Then build FAISS index:
```bash
python src/build_knowledge_db.py
```

---

## Running the Application

### 1. Start Backend
```bash
python app/main.py
```

### 2. Optional: Start UI
Navigate to `app/ui/` and run Streamlit or React app:
```bash
streamlit run app/ui/app.py
```

---

## Example Usage

**Input:** Prescription image with  
```
Metformin 500mg, twice daily
```

**Output:**
```json
{
  "Medicines": ["Metformin"],
  "Predicted Disease": ["Type 2 Diabetes Mellitus"],
  "Symptoms": ["High blood sugar", "Frequent urination"],
  "Side Effects": ["Nausea", "Diarrhea", "Abdominal discomfort"],
  "Health Tips": [
    "Maintain a low-sugar diet",
    "Exercise regularly",
    "Monitor blood sugar levels"
  ]
}
```

---

## Tech Stack

- **Language:** Python 3  
- **Models:** BioBERT, FAISS  
- **OCR:** PaddleOCR / Tesseract  
- **Framework:** FastAPI / Flask  
- **UI:** Streamlit or React  

---

## Future Improvements

- Expand dataset coverage  
- Add multi-lingual prescription support  
- Integrate voice prescription reading  
- Deploy as a cloud-based web service  
