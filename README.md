# SRNet Steganography Detection

> **Summary:** End-to-end deep learning steganalysis using SRNet with real evaluation metrics and deployment via FastAPI/Streamlit.

## 1. Problem Statement
Digital steganography—hiding payloads within innocent-looking images—allows covert communication for malicious actors. Traditional detection fails against adaptive algorithms like WOW and HILL because they embed data in noisy regions. Deep learning is required to learn these complex, non-linear artifact patterns that manual feature extraction misses.

## 2. Approach & Methodology
This project implements **SRNet (Steganalysis Residual Network)**, a specialized CNN designed to suppress image content and amplify steganographic noise.
- **Preprocessing**: Uncompressed images are cropped to 256x256 to preserve high-frequency noise.
- **Model**: SRNet architecture (layers 1-7 for noise extraction, 8-12 for classification).
- **Evaluation**: Prioritized **Recall** to minimize missed detections in a security context.

## 3. Dataset
- **Sources**: **BossBase v1.01** and **BOWS2** (Standard steganalysis datasets).
- **Generation**: Cover/Stego pairs generated using **LSB, WOW, and HILL** algorithms at 0.2-0.4 bits per pixel (bpp).
- **Balance**: Strictly balanced dataset (50% Cover, 50% Stego) to prevent class bias.

## 4. Training Configuration
- **Epochs**: ~50 (with early stopping).
- **Batch Size**: 32 (optimized for GPU memory).
- **Optimizer**: Adam ($lr=1e-3$ with reducing schedule).
- **Split**: 80% Training / 20% Validation.
- **Loss**: Cross-Entropy Loss combined with class weighting.

## 5. Results & Evaluation
Accuracy is secondary in security; missing a threat (False Negative) is unacceptable. Thus, **Recall** is the primary metric.

| Metric | Score | Context |
| :--- | :--- | :--- |
| **Recall** | **~84%** | Measures ability to detect hidden payloads. |
| **F1-Score** | **~0.63** | Balances precision and recall in noisy scenarios. |

**Observation**: The model is highly sensitive but prone to false positives on unseen sources (distribution shift).

## 6. Key Learnings
- **Distribution Shift is Critical**: A model trained on WOW embeddings significantly degrades when tested on S-UNIWARD, proving the need for diverse training data.
- **Zero-Day Vulnerability**: Detecting algorithms "not seen" during training remains an open research challenge; generalization is limited.
- **Dataset Quality**: JPG compression destroys steganographic signals; training must strictly use uncompressed (PGM/TIF) formats.
- **Recall/Precision Trade-off**: Adjusting the decision threshold to boost Recall inevitably lowers Precision, a necessary trade-off for security tools.

## 7. Tech Stack
- **Deep Learning**: PyTorch, SRNet implementation
- **Backend API**: FastAPI, Uvicorn
- **Frontend**: Streamlit
- **Data**: NumPy, Pandas, OpenCV

## 8. Project Structure
```bash
├── data/               # Scripts for dataset generation (cover/stego pairs)
├── models/             # SRNet architecture definition
├── training/           # Training loops and validation scripts
├── deployment/
│   ├── api.py          # FastAPI backend
│   └── app.py          # Streamlit dashboard
├── requirements.txt    # Project dependencies
└── README.md
```

## 9. How to Run

1. **Clone the Repository**
   ```bash
   git clone [Your Repository URL]
   cd SRNet-Steganography
   ```

2. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the API (Backend)**
   ```bash
   uvicorn deployment.api:app --reload
   ```

4. **Run the Dashboard (Frontend)**
   ```bash
   streamlit run deployment/app.py
   ```

## 10. Contact
- **Email**: Saiganesh191919@gmail.com
- **GitHub**: [github.com/ganesh-sanam](https://github.com/ganesh-sanam)
- **LinkedIn**: [linkedin.com/in/sai-ganesh-a31157335](https://www.linkedin.com/in/sai-ganesh-a31157335)
