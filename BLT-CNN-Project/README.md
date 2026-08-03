<div align="center">

```
██████╗ ██╗  ████████╗      ██████╗███╗   ██╗███╗   ██╗
██╔══██╗██║  ╚══██╔══╝     ██╔════╝████╗  ██║████╗  ██║
██████╔╝██║     ██║        ██║     ██╔██╗ ██║██╔██╗ ██║
██╔══██╗██║     ██║        ██║     ██║╚██╗██║██║╚██╗██║
██████╔╝███████╗██║        ╚██████╗██║ ╚████║██║ ╚████║
╚═════╝ ╚══════╝╚═╝         ╚═════╝╚═╝  ╚═══╝╚═╝  ╚═══╝
```

# 🧬 BLT-CNN: Antibiotic Resistance Predictor

### *When every second counts, your model shouldn't guess.*

<br>

[![Accuracy](https://img.shields.io/badge/Accuracy-93.34%25-brightgreen?style=for-the-badge&logo=checkmarx&logoColor=white)](https://github.com/man-ra/BLT-CNN-Project)
[![AUC](https://img.shields.io/badge/AUC-98.55%25-blue?style=for-the-badge&logo=tensorflow&logoColor=white)](https://github.com/man-ra/BLT-CNN-Project)
[![F1](https://img.shields.io/badge/F1_Score-94.18%25-orange?style=for-the-badge&logo=pytorch&logoColor=white)](https://github.com/man-ra/BLT-CNN-Project)
[![Dataset](https://img.shields.io/badge/Dataset-CARD_4005_seqs-purple?style=for-the-badge&logo=databricks&logoColor=white)](https://card.mcmaster.ca/)
[![Python](https://img.shields.io/badge/Python-3.9+-yellow?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-red?style=for-the-badge)](LICENSE)

<br>

> **BLT-CNN** is a novel hybrid deep learning architecture that predicts antibiotic resistance  
> from raw DNA sequences — combining entropy-guided patching, local motif detection, and  
> global sequence context in a single end-to-end trainable model.

<br>

---

</div>

## 🌍 Why This Matters

> *"Antimicrobial resistance is predicted to kill 10 million people per year by 2050 — more than cancer."*  
> — UN Interagency Coordination Group on AMR

Traditional resistance testing takes **24–72 hours**. Patients die waiting.  
BLT-CNN predicts resistance from a DNA sequence in **milliseconds**.

<br>

---

## ⚡ Architecture — Three Engines, One Model

```
                    DNA Sequence (300 nucleotides)
                            │
                    ┌───────▼───────┐
                    │   Embedding   │  5-token vocab → 64-dim vectors
                    └───────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              │                           │
    ┌─────────▼──────────┐    ┌──────────▼──────────┐
    │  BLT Entropy       │    │   1D CNN             │
    │  Patcher           │    │   Local Motifs       │
    │                    │    │                      │
    │  H(i) > 1.5 bits   │    │  Conv(256, k=9)      │
    │  → 3-nt patch      │    │  Conv(128, k=9)      │
    │  H(i) ≤ 1.5 bits   │    │  MaxPool(2)          │
    │  → 12-nt patch     │    │                      │
    │                    │    └──────────┬──────────┘
    │  Output: 64-dim    │               │
    └─────────┬──────────┘    ┌──────────▼──────────┐
              │               │  Transformer         │
              │               │  Encoder             │
              │               │                      │
              │               │  4 layers, 8 heads   │
              │               │  GELU, sinusoidal PE │
              │               │  Output: 128-dim     │
              │               └──────────┬──────────┘
              │                          │
              └──────────┬───────────────┘
                         │  Concatenate [128 + 64] = 192-dim
                ┌────────▼────────┐
                │  Fusion MLP     │  192 → 256 → 128
                │  LayerNorm      │  + Dropout
                └────────┬────────┘
                         │
                ┌────────▼────────┐
                │  Classifier     │  128 → 3
                │  Sigmoid        │  Multi-label output
                └────────┬────────┘
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
     Methicillin   Ciprofloxacin   Vancomycin
       99.8%           0.6%           0.0%
      RESISTANT     SUSCEPTIBLE    SUSCEPTIBLE
```

<br>

---

## 📊 Results

<div align="center">

| Metric | Score |
|:------:|:-----:|
| 🎯 Accuracy | **93.34%** |
| 📈 AUC | **98.55%** |
| ⚖️ F1 Score | **94.18%** |
| 🔬 Precision | **95.57%** |
| 📡 Recall | **93.35%** |
| 🧮 Parameters | **944,387** |

</div>

### Per-Class Performance

| Antibiotic | Samples | Precision | Recall | F1 | Status |
|:----------:|:-------:|:---------:|:------:|:--:|:------:|
| 🔵 Methicillin | 3,612 | 0.9761 | 0.9852 | **0.9806** | ✅ Excellent |
| 🟡 Ciprofloxacin | 283 | 0.8857 | 0.5849 | **0.7045** | ⚠️ Moderate |
| 🔴 Vancomycin | 171 | 0.6500 | 0.5417 | **0.5909** | 🔧 Improving |

<br>

---

## 🧬 The Three Antibiotics

<table>
<tr>
<td align="center" width="33%">

### 💉 Methicillin
**Beta-lactam antibiotic**

Targets *S. aureus* (MRSA)  
Resistance gene: `mecA`  
*Most common hospital-acquired infection worldwide*

</td>
<td align="center" width="33%">

### 💊 Ciprofloxacin
**Fluoroquinolone antibiotic**

Targets *E. coli*, *Klebsiella*  
Resistance genes: `qnr`, `gyrA`  
*Most prescribed broad-spectrum antibiotic*

</td>
<td align="center" width="33%">

### ⚠️ Vancomycin
**Glycopeptide antibiotic**

Targets *Enterococcus* (VRE)  
Resistance genes: `vanA`, `vanB`  
*Last-resort antibiotic — critical priority*

</td>
</tr>
</table>

<br>

---

## 🚀 Quick Start

### 1. Clone & Install
```bash
git clone https://github.com/man-ra/BLT-CNN-Project.git
cd BLT-CNN-Project
pip install -r requirements.txt
```

### 2. Run the 3D Web UI
```bash
# Start the FastAPI backend
python app_api.py

# Then open blt_cnn_ui.html in your browser
# → Real predictions with 3D DNA helix animation
```

### 3. Run Streamlit App
```bash
python -m streamlit run app.py
# Opens at http://localhost:8501
```

### 4. Train from Scratch
```bash
# Process CARD database
python scripts/process_card.py

# Train BLT-CNN
python scripts/train_card.py
```

<br>

---

## 🗂️ Project Structure

```
BLT-CNN-Project/
│
├── 🧠 src/
│   ├── models/
│   │   ├── blt_patcher.py      ← Entropy-guided dynamic patching
│   │   ├── cnn1d.py            ← 1D CNN local motif detection
│   │   ├── transformer.py      ← Transformer encoder
│   │   └── blt_cnn.py          ← Hybrid fusion model ⭐
│   ├── data/
│   │   ├── dataset.py          ← CARD data loader
│   │   └── encoding.py         ← Nucleotide encoder
│   └── training/
│       ├── trainer.py          ← Training loop
│       ├── losses.py           ← Loss functions
│       └── metrics.py          ← Evaluation metrics
│
├── 🔬 scripts/
│   ├── process_card.py         ← CARD database processor
│   ├── train_card.py           ← Main training script
│   └── evaluate.py             ← Evaluation script
│
├── 🌐 Frontend/
│   ├── blt_cnn_ui.html         ← 3D animated web UI ✨
│   ├── app.py                  ← Streamlit app
│   └── app_api.py              ← FastAPI backend
│
├── 📊 results/
│   └── card_results.json       ← Training results
│
└── 🤖 models/
    └── blt_cnn_card_best.pth   ← Best trained model (epoch 18)
```

<br>

---

## 🔬 How BLT Entropy Patching Works

Unlike fixed tokenization, BLT dynamically adjusts resolution based on **information content**:

```
DNA:  A T G A A A C G T A T C G G A A T T C G ...
      │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ │
Ent:  1.2 1.1 2.1 2.3 1.9 2.2 1.0 1.0 1.1 1.2
       ↓   ↓    ↓   ↓   ↓   ↓   ↓   ↓   ↓   ↓
      [  large  ][small][small][  large  ][large]
      12-nt patch 3-nt  3-nt  12-nt patch
      conserved   hotspot      conserved
```

- **High entropy (>1.5 bits)** → 3-nucleotide patches → captures mutation hotspots
- **Low entropy (≤1.5 bits)** → 12-nucleotide patches → efficiently compresses conserved regions

This mirrors biology: resistance mutations cluster in specific variable regions while flanking sequences remain conserved.

<br>

---

## 📦 Dependencies

```
torch>=2.0
numpy
scikit-learn
fastapi
uvicorn
streamlit
biopython
pandas
matplotlib
```

<br>

---

## 📄 Citation

If you use this work, please cite:

```bibtex
@misc{bltcnn2026,
  title     = {BLT-CNN: A Hybrid Deep Learning Architecture for
               Antibiotic Resistance Prediction from Genomic Sequences},
  author    = {Mantasha},
  year      = {2026},
  url       = {https://github.com/man-ra/BLT-CNN-Project},
  note      = {B.Tech Research Project}
}
```

<br>

---

## 🙏 References

- Pagnoni et al. (2024) — *Byte Latent Transformer*, Meta AI
- Alcock et al. (2023) — *CARD 2023*, Nucleic Acids Research
- Arango-Argoty et al. (2018) — *DeepARG*, Microbiome
- Vaswani et al. (2017) — *Attention Is All You Need*, NeurIPS

<br>

---

<div align="center">

**Built with 🧬 by Mantasha**

*B.Tech 3rd Year · Antibiotic Resistance Research*

⭐ Star this repo if you found it useful

---

*"The bacteria are evolving. So should our tools."*

</div>
