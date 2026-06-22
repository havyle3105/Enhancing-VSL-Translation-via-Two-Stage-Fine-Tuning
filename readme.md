# Speech Recognition and Large Language Model Application for Vietnamese Sign Language Automatic Translation to Support the Hearing Impaired 🇻🇳

**Graduation Thesis - Falcuty of E-Commerce**

**Danang University of Economics**

---
## Table of Contents
- [Abstract](#abstract)
- [Introduction](#introduction)
- [Dataset](#dataset)
- [Methodology](#methodology)
  - [Architecture](#architecture)
  - [Experimental Setup](#experimental-setup)
- [Results](#results)
- [Installation & Usage](#installation--usage)

---

## Abstract

This project originates from the practical communication challenges faced by hearing impaired staff and customers at **Angel Coffee (Danang)**. This sytem is desgined to eliminate this communication barrier through an automated conversion pipeline:

**Vietnamese Speech (Input) ➡️ Vietnamese Text ➡️ Sign Language Syntax Text (Output)**

The final output is represented in **VSL Gloss** (the correct grammatical representation of Vietnamese Sign Language), which also serves as standardized input data for future 3D Avatar animation models.

## Introduction
This repository contains the core codebase and research framework for developing an end-to-end translation system tailored for the hearing impaired. The core contributions of this repository include:
1. **Data Resources:** Construction of a specialized bilingual dataset for the F&B (coffee service) domain through real-world collection and data augmentation leveraging LLMs (ChatGPT, Gemini).
2. **Optimized Model** Proposal of a **2-stage Fine-tuning** training pipleline that enables the model to grasp general grammar while simultaneously acquiring deep comprehension of specialized domain vocabulary.
3. **Integrated System** Development of a complete pipeline seamlessly combining Automatic Speech Recognition (ASR) and Machine Translation (MT).

## Dataset
The project utilizes a combination of foundational data and domain-specific data:

* **`Corpus-Vie-VSL-10K`**: Foundational dataset (10,000 sentences) inherited previous research
* **`Coffee-Vie-VSL`**: Original dataset (116 sentences) collected from real-world interactions at the coffee shop
* **`Combined-Augmented-Coffee`**: Comprehensive dataset after augmentation techniques (1,691 sentences)
---
## Methodology
### Architecture
The system operates on a pipeline architecture consisting of two primary modules:

* **Module 1 (Speech-to-Text):** Utilized the **PhoWhisper-small** model to convert spoken Vietnamese into text with high accuracy and low Word Error Rate (WER).
* **Module 2 (Machine Translation):** Utilized the **ViT5-base** model to translate standard Vietnamese text into Sign Language syntax text (VSL Gloss)

![Architecture Diagram](Picture1.jpg)
*(General data flow diagram of the system)*
---
### Experimental Setup
The project is structured into 6 sequential notebooks, representing the end-to-end research workflow from data processing to deployment:

| No. | File Name | Functional Description |
| :--- | :--- | :--- |
| **01** | `01_Data_Analysis_and_Augmentation_Eval.ipynb` | **Preprocessing & Analysis:** Statistical analysis of original data (116 sentences), data augmentation via ChatGPT/Gemini (up to 1,691 sentences), and quality evaluation (TTR, Cosine Similarity)|
| **02** | `02_FineTuning01_Baseline_Train_BARTpho_General10k.ipynb` | **Baseline 1 Training:** Fine-tuning the **BARTpho** model on the general 10k dataset to establish a performance benchmark |
| **03** | `03_FineTuning01_Baseline_Train_ViT5_General10k.ipynb` | **Baseline 2 Training:** Fine-tuning and performance comparison between **ViT5-base** and **ViT5-large** on the general dataset|
| **04** | `04_FineTuning02_DomainAdapt_ViT5_Coffee_Augmented.ipynb` | **Strategy A:** 2-stage Fine-tuning on the ViT5-base model utilizing the augmented Coffee dataset |
| **05** | `05_TransferLearning_ViT5_LoRA.ipynb` | **Strategy B (Comparison):** Application of **Transfer Learning** combined with **LoRA** on the ViT5-base model to evaluate against Strategy A|
| **06** | `06_Inference_Demo_System.ipynb` | **Deployment:** Integration of PhoWhisper and the optimal ViT5 mdoel into a Web App interface (Gradio) for real-world product demonstration|

---
## Results
Following extensive experimentation and comparative analysis, the research yielded the following key findings:

* **Optimal Model:** **ViT5-base** utilizing the **2-stage Fine-tuning** strategy.
    * *Stage 1.* General grammar acquisition from the 10k dataset
    * *Stage 2.* Domain adaptation for specialized coffee vocabulary
* **Comparative Performance:** The 2-stage Fine-tuning strategy demonstrated superior results compared to single-stage fine-tuning and Transfer Learning alternatives.

|Model Strategy | BLEU Score | Wer (Word Error Rate)|
|-------|-----------------------|----------------------|
| Transfer Learning (LoRA) | ~90.00% | - |
| **Proposed (2-stage Fine-tuning)** | **95.75%** | **2.70%** |

---
## Installation & Usage
To test and experience the system demo, please execute notebook **06** (`06_Inference_Demo_System.ipynb`) via Google Colab:

1. **Preparation:** Upload the required notebook and associated data files to your Google Drive
2. **Environment Configuration:** Set the Colab Runtime type to **T4GPU**
3. **Execution:** Execute all cells sequentially (`Runtime` -> `Run all`)
4. **Interactive Demo:** Navigate to the `Gradio Public URL` provided at the end of the notebook output to launch the user interface. You can input text directly or use voice recording to text the translation pipeline.
