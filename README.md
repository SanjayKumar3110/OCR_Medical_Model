# OCR Medical Model – MediScan

## Overview

This module implements a **Custom Convolutional Neural Network (CNN)** for recognizing handwritten medicine names from doctor prescriptions. Unlike generic OCR tools, this model is specifically trained on the **Doctors Handwritten Prescription BD Dataset** to classify prescription text into 78 distinct medicine classes.

It is a core component of the **MediScan – AI Prescription Analyzer** project, designed to automate the digitization of medical records.

---

## Features

*   **Custom CNN Architecture**: A deep learning model built with TensorFlow/Keras for image classification.
*   **Specific Domain Training**: Trained on real-world handwritten medical prescriptions.
*   **Preprocessing Pipeline**: Automated image resizing (64x64), normalization, and label encoding.
*   **Comprehensive Evaluation**: Includes accuracy metrics, confusion matrices, and classification reports.
*   **EasyOCR Integration**: Includes `easyocr` for potential auxiliary text extraction tasks.

---

## Folder Structure

```
OCR_Medical_Model/
│── data/                      # Dataset directory (Training/Testing images & labels)
│── ocr/
│   │── ocr_script.ipynb       # Main Jupyter Notebook for training, testing & evaluation
│   │── class_labels.json      # JSON file containing the 78 medicine class labels
│   │── sample_images/         # Example prescription images
│   └── models/                # Directory where trained models (.h5, .keras) are saved
│── requirements.txt           # Python dependencies
└── README.md                  # Project documentation
```

---

## Tech Stack

*   **Deep Learning**: TensorFlow, Keras
*   **Computer Vision**: OpenCV, EasyOCR
*   **Data Handling**: Pandas, NumPy
*   **Evaluation**: Scikit-learn (Confusion Matrix, Classification Report)
*   **Visualization**: Matplotlib

---

## How It Works

1.  **Data Loading**: Reads images and their corresponding labels from the dataset CSVs.
2.  **Preprocessing**:
    *   Images are resized to **64x64 pixels**.
    *   Pixel values are normalized to the [0, 1] range.
    *   Labels are encoded using `LabelEncoder` and converted to categorical vectors.
3.  **Model Architecture**:
    *   **Conv2D Layers**: Extract features from images.
    *   **MaxPooling2D**: Reduces spatial dimensions.
    *   **Dense Layers**: Fully connected layers for classification.
    *   **Dropout**: Prevents overfitting.
    *   **Output Layer**: Softmax activation with 78 units (one for each medicine class).
4.  **Training**: The model is trained for 20 epochs using the Adam optimizer.
5.  **Evaluation**: The model is tested on a separate test set to ensure generalization.

---

## Installation

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/SanjayKumar3110/OCR_Medical_Model.git
    ```

2.  **Install dependencies**:

    ```bash
    pip install -r requirements.txt
    ```

    *Note: Ensure you have a Python environment (recommended 3.10+) set up.*

---

## Usage

### Training and Testing the Model

1.  Navigate to the `ocr` directory:
    ```bash
    cd ocr
    ```
2.  Open the Jupyter Notebook:
    ```bash
    jupyter notebook ocr_script.ipynb
    ```
3.  Run the cells sequentially to:
    *   Load and preprocess the data.
    *   Train the CNN model.
    *   Evaluate performance on the test set.
    *   Save the trained model to the `models/` directory.

---

## Dataset
The dataset used for training is publicly available on **Kaggle**:
> 
The project uses the **[Doctors Handwritten Prescription BD Dataset](https://www.kaggle.com/datasets/mamun1113/doctors-handwritten-prescription-bd-dataset)** from Kaggle, which contains:
*   **Training Data**: Images of handwritten medical words.
*   **Labels**: CSV files mapping images to medicine names.

---

## Future Work

*   **Data Augmentation**: To improve model robustness against variations in handwriting.
*   **Hyperparameter Tuning**: Optimizing learning rates and batch sizes for better accuracy.
*   **Integration**: Connecting this module with the main MediScan backend for real-time predictions.
*   **Transformer Models**: Experimenting with Vision Transformers (ViT) for potentially higher accuracy.

