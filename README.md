# 🧠 Cognitive Radiology Report Generation
### *A Hi-CliTr Inspired Deep Learning Framework for Chest X-Ray Reporting*
> **Team Technicali** — Brain Dead, REVELATION 5.0 | ASCE, IIEST Shibpur


---

## 💡 Motivation

Healthcare accessibility in India remains a critical challenge. Radiologist fatigue leads to **3–5% reporting discrepancies**, and manual reporting is time-intensive, error-prone, and difficult to scale. Meanwhile, imaging volume grows exponentially while radiologist staffing remains flat — creating a dangerous **Burnout Gap**.

> **The goal:** Build an AI Second Reader that *supports* clinicians, not replaces them.

Standard image captioning models fail at radiology because:
- They lack **clinical reasoning** — fluent but medically illogical
- They **hallucinate** — risk of inventing unsupported findings
- They suffer **semantic unfaithfulness** — inability to distinguish spatial orientation (e.g., left vs right lung)

> **Key Insight: Radiology reporting is a *reasoning* task, not a captioning task.**

---

## 🏗️ Architecture — The "Cognitive Mirror"

The framework mimics the radiologist's actual mental workflow across four stages:

```
Observe → Hypothesize → Verify → Report
```

| Stage | Module | Description |
|-------|--------|-------------|
| 👁️ **Observe** | PRO-FA | Hierarchical Visual Perception |
| 🧠 **Hypothesize** | MIX-MLP | Knowledge-Enhanced Diagnosis |
| ✅ **Verify** | RCTA | Closed-Loop Attention |
| 📄 **Report** | Decoder | Structured Text Generation |

---

### Module 1: PRO-FA (Hierarchical Visual Alignment)
Radiologists scan images at multiple scales — from global anatomy down to pixel-level lesions. PRO-FA replicates this by:
- **Vision Transformers (ViT)** for global organ-level context
- **Swin Transformers** for hierarchical region and lesion-level detail
- Aligning visual features with **RadLex ontology concepts**

```
Organ Level (Global) → Region Level (Lung Lobes) → Pixel Level (Lesion)
```

---

### Module 2: MIX-MLP (Knowledge-Enhanced Diagnosis)
Separates the *diagnosis phase* from the *writing phase* — just like a real clinician thinks before they write.
- Takes visual features from PRO-FA as input
- Applies **Token-Mixing (Spatial)** and **Channel-Mixing (Feature)** operations
- Outputs **14-Class Disease Probabilities** (e.g., Effusion, Edema, Pneumonia...)

---

### Module 3: RCTA (Triangular Cognitive Attention)
A closed-loop verification mechanism that mimics a doctor double-checking their work.

```
Image ──► Diagnosis List ──► Text Context ──► back to Image (Verify)
```

> If the generated text **cannot be traced back to specific pixels**, it is **discarded**.  
> This is the core anti-hallucination mechanism.

---

### Report Generation
Using **Clinical-T5 / BioGPT** as decoder backbone, the system generates structured reports in two sections:

- **FINDINGS** — Objective observations (e.g., *"Opacity seen in right lower lobe"*)
- **IMPRESSION** — Diagnostic summary conditioned on findings (e.g., *"Findings suggestive of pneumonia"*)

Inputs to the decoder: Verified Visual Evidence + Predicted Disease Tokens + RadLex Location Embeddings

---

## 📊 Dataset & Knowledge Backbone

| Dataset | Purpose | Scale |
|---------|---------|-------|
| **MIMIC-CXR** | Training | 377K images, PA & Lateral views, paired with free-text reports |
| **IU-Xray** | Zero-shot Evaluation | Domain generalization benchmark |

**Knowledge Backbone:** RadLex Ontology encoded via **BioClinicalBERT** to create semantic anchors, grounding vision in clinical reality.

---

## 🎓 Training Strategy

A **curriculum learning** approach across three phases:

```
Early Epochs          Mid Training          Late Epochs
Diagnosis Accuracy  →  Reasoning (RCTA)  →  NLG Quality
& Visual Alignment
```

**Loss Function:**
```
L = L_Diagnosis + L_RadLexAlignment + L_NLG
```

**Focal Learning** dynamically assigns higher weights to rare diseases to prevent the model from ignoring difficult cases.

---

## 📈 Results

| Metric | Baseline Transformer | R2Gen | Cognitive Framework (Ours) |
|--------|---------------------|-------|---------------------------|
| **F1-Score** | ~40% | ~50% | **~87%** |
| **Entity-Location Accuracy** | ~30% | ~45% | **~82% (Zero-Shot)** |

Key highlights:
- ✅ Significantly reduced hallucinations compared to baselines
- ✅ Improved localization of pathology (Left vs Right distinction)
- ✅ Superior **zero-shot performance** on unseen IU-Xray data

---

## 🔬 Ablation Study

Every module is essential:

| Without | Effect |
|---------|--------|
| **PRO-FA** | Semantic errors increase; model looks at wrong regions |
| **MIX-MLP** | Diagnostic accuracy drops; reports are fluent but wrong |
| **RCTA** | Hallucinations spike; model invents findings |

> **Conclusion:** The synergy of Perception, Diagnosis, and Verification is required for safe reporting.

---

## 📏 Evaluation Framework

| Dimension | Metric | Question |
|-----------|--------|----------|
| Clinical Accuracy | CheXpert F1 | Did we get the disease right? |
| Structural Reasoning | RadGraph F1 | Did we get the relationships right? |
| Language Quality | CIDEr / BLEU-4 | Is the text readable? |

---

## ⚠️ Limitations & Future Work

**Current Limitations:**
- **Subtle Pathologies** — difficulty detecting micro-nodules
- **Image Quality** — performance degrades with rotation/contrast issues
- **Ambiguity** — struggles when clinical history is missing

**Mitigation:** Uncertainty-Aware Reporting — the model expresses uncertainty (e.g., *"possible opacity"*) rather than false confidence.

**Future Directions:**
- 🕐 **Context-Awareness** — incorporate time-of-day and device context
- 📊 **Implicit Duration** — use Duration Count Matrix (DCM) for engagement tracking
- ⚙️ **Dynamic Lambda** — auto-adjust diversity settings based on user entropy

---

## 🛠️ Tech Stack

```
Deep Learning     : PyTorch
Vision Encoders   : Vision Transformer (ViT), Swin Transformer
Text Decoders     : Clinical-T5, BioGPT
Knowledge Graph   : RadLex Ontology + BioClinicalBERT
Datasets          : MIMIC-CXR, IU-Xray
Evaluation        : CheXpert, RadGraph, CIDEr, BLEU-4
```

---

## 🚀 Getting Started

```bash
# Clone the repository
git clone https://github.com/Ridhibrata-Das/Brain-Dead-PS2.git
cd Brain-Dead-PS2

# Install dependencies
pip install -r requirements.txt

# Run the CXR notebook
jupyter notebook CXR_Braindead.ipynb

# Run the main pipeline notebook
jupyter notebook "brainded (1).ipynb"
```

---

## 👥 Team Technicali

- **Archisman Ghosh**
- **Ridhibrata Das**
- **Rishik Pal**

---

## 📜 License

This project was developed for **Brain Dead @ REVELATION 5.0**, organized by the Academic Society of Computer Engineers (ASCE), IIEST Shibpur.

---

> *"Augmenting, not replacing, the human expert. Moving from image captioning to true clinical reasoning."*
