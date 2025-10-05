# MediScan – AI Prescription Analyzer

## Overview

MediScan is an AI-powered system designed to automatically analyze medical prescriptions (images or text), extract medicine information, and provide insights including:

- Predicted diseases related to prescribed medicines  
- Associated symptoms and side effects  
- Preventive health tips
- Doctor suggestions 

The project uses a **Hybrid RAG (Retrieval-Augmented Generation)** approach, combining a fine-tuned **Named Entity Recognition (NER)** model with a **retrieval-based medical knowledge base** for accurate and updatable predictions.

---

## Features

- **OCR Module**: Extracts text from prescription images using Tesseract or PaddleOCR.  
- **NER Model**: Fine-tuned BioBERT model identifies medicines, symptoms, and dosages.  
- **RAG System**: Retrieves disease names, side effects, and health tips from a structured medical database.  
- **End-to-End Pipeline**: Handles both image and text input.  
- **API & UI**: Provides REST API and optional web interface for uploads and results visualization.  

---

---

## High-Level- system architecture

[User] -> Web/Mobile App (chat + upload image) 
   -> Backend API
       -> Image preprocessing (denoise, binarize)
       -> OCR / Handwriting Recognition -> raw text
       -> Prescription Parsing (NER + relation extraction)
       -> Structured Record (patient info, meds[], diagnoses[])
       -> RAG Retrieval:
            - Vector DB (embeddings of medical docs, drug monographs)
            - Retriever (FAISS/Milvus/Weaviate)
            - LLM (local quantized/inference API or OpenAI)
            - RAG response generator (answer + citations)
       -> Nearby Doctors Service (Places API / OSM + filter by specialty)
       -> Post-processing / Safety checks / Logging
   -> UI: chat, annotated prescription, medication summary, map + contact

---

## Project Structure

```
MediScan/
  │── data/                         # Folder to store dataset, images ans sample prescriptions
  │                           
  │── models/                       # Trained models (OCR, Disease, Advice, DoctorFinder)
  │   │── ocr_model.py
  │   │── disease_advice_model.py
  │   │── near_doctors_model.py
  │
  │── knowledge_base/               # Medical KB + embeddings
  │
  │── src/
  │   │── preprocessing/            # OCR + cleaning scripts
  │   │── rag_pipeline/             # Vector DB, retrieval, response generation
  │   │── chat_interface/           # Streamlit/Flask chat system
  │   │── utils/                    # Helpers (API calls, logging, config)
  │
  │── tests/                        # Unit tests
  │── app/
  │    ├── main.py                  # FastAPI or Flask backend
  │    └── ui/                      # Streamlit/React UI for uploads
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
git clone https://github.com/sanjaykumar3115/MediScan.git
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
  "patient": {"age":"45","gender":"M","date":"2025-09-20"},
  "meds":[
    {"drug_name":"Amoxicillin","purpose":"Antibiotic","dose":"500 mg","frequency":"TDS","duration":"5 days","notes":"Take after food","confidence":0.92}
  ],
  "likely_conditions":["Tonsillitis"],
  "advice":"This looks like a short antibiotic course for suspected bacterial tonsillitis. If fever or breathing difficulty occurs, see a doctor immediately. I am not a doctor; consult a licensed healthcare provider.",
  "sources":[{"id":"doc_01","title":"Amoxicillin monograph","url":"..."}],
  "nearby_doctors":[{"name":"Dr. X","specialty":"ENT","distance_km":1.2,"phone":"+91..."}]
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
