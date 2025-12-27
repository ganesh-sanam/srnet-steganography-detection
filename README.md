# SRNet Steganography Detection

**End-to-end deep learning steganalysis pipeline using SRNet to detect hidden payloads in digital images, prioritizing recall for security.**

## 1. Problem Statement
Digital steganography allows malicious actors to covertly transmit malware or data by hiding it within innocent-looking images. Traditional steganalysis fails against modern adaptive algorithms (like WOW and HILL) that hide data in noisy image regions. This project utilizes deep learning (SRNet) to learn these complex, non-linear noise artifacts that standard tools miss.

## 2. Approach & Methodology
We implemented **SRNet (Steganalysis Residual Network)**, a specialized CNN architecture designed to suppress image content and amplify steganographic noise residuals.
- **Preprocessing**: High-frequency feature extraction via residual layers (layers 1-7).
- **Classification**: Deep classifier (layers 8-12) to distinguish Cover vs. Stego.
- **Evaluation Strategy**: Prioritized **Recall** (Sensitivity) to minimize False Negatives, ensuring potential threats are flagged.

## 3. Dataset
To ensure realistic evaluation, we used industry-standard sourced datasets:
- **Sources**: **BossBase v1.01** and **BOWS2** (Standard steganalysis benchmarks).
- **Generation Method**: created 50% Stego images using **LSB, WOW, and HILL** algorithms at payloads of 0.2–0.4 bits per pixel (bpp).
- **Input**: 256x256 grayscale images (cropped from raw uncompressed sources).
- **Class Balance**: Strictly balanced **50% Cover / 50% Stego** to prevent model bias.

## 4. Training Configuration
- **Epochs**: 50 (with Early Stopping)
- **Batch Size**: 32
- **Optimizer**: Adam ($lr=1e-3$ -> $1e-5$ schedule)
- **Loss Function**: Weighted Cross-Entropy Loss
- **Split**: 80% Training / 20% Validation

## 5. Results & Evaluation
We prioritized detecting threats (Recall) over excluding safe images (Precision).

| Metric | Score | Why it matters |
| :--- | :--- | :--- |
| **Recall** | **~84%** | Critical: Ensuring we catch ~84% of all hidden payloads. |
| **F1-Score** | **~0.63** | Balances the inevitable increase in false positives when maximizing recall. |
| **Accuracy** | *Secondary* | Less relevant in security contexts where missed detections are costly. |

## 6. Key Learnings
- **Distribution Shift**: Models trained on one algorithm (e.g., WOW) struggle to detect others (e.g., HILL) without retraining, highlighting the need for ensemble generalizers.
- **Dataset Quality**: Standard JPEG compression destroys steganographic artifacts; training must strictly use uncompressed (PGM/TIFF) formats.
- **Zero-Day Attacks**: Generalization to completely unseen steganography methods remains the biggest challenge in the field.

## 7. Tech Stack
- **Deep Learning**: PyTorch, SRNet
- **Backend**: FastAPI, Uvicorn
- **Frontend**: Streamlit
- **Tools**: OpenCV, NumPy, Pandas

## 8. Project Structure
```bash
├── data/               # Dataset generation scripts (matlab/python)
├── models/             # SRNet PyTorch definition
├── training/           # Train/Val loops
├── deployment/
│   ├── api.py          # FastAPI service
│   └── app.py          # Streamlit UI
├── requirements.txt    # Python dependencies
└── README.md
```

## 9. How to Run

**Step 1: Clone the Repository**
```bash
git clone [Your Repository URL]
cd SRNet-Steganography
```

**Step 2: Install Dependencies**
```bash
pip install -r requirements.txt
```

**Step 3: Run the Backend API**
```bash
uvicorn deployment.api:app --reload
```

**Step 4: Run the Frontend Dashboard** (in a new terminal)
```bash
streamlit run deployment/app.py
```

## 10. Contact
- **Email**: Saiganesh191919@gmail.com
- **GitHub**: [github.com/ganesh-sanam](https://github.com/ganesh-sanam)
- **LinkedIn**: [linkedin.com/in/sai-ganesh-a31157335](https://www.linkedin.com/in/sai-ganesh-a31157335)
