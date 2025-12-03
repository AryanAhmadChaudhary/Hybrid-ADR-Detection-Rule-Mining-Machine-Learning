# Hybrid ADR Detection: Combining Rule Mining with Machine Learning

This project explores a hybrid approach for identifying **Adverse Drug Reactions (ADR)** from user-generated text by integrating:

- **Apriori-based lexical rules** (interpretable symbolic patterns)  
- **Classical machine learning models** (Logistic Regression / SVM)  
- A **confidence-gated decision strategy**, where high-precision rules override ML predictions  

The aim is to study how symbolic patterns and statistical models can complement each other in biomedical text classification, improving interpretability and reliability.

---

## Problem Overview

Automatically detecting ADR from text is an important task for healthcare monitoring and pharmacovigilance.  
However:

- Pure rule-based systems are precise but brittle
- Pure ML systems are **flexible but may miss rare ADR indicators**

This project investigates a lightweight hybrid ADR detection architecture that leverages the strengths of both approaches.

---

## Methodology

### **1. Text Preprocessing**
- Lowercasing  
- Tokenization  
- Stopword removal  
- TF–IDF (simple, interpretable baselines)

### **2. Rule Mining (Apriori)**
- Frequent token patterns mined using Apriori  
- Extracted single-word ADR indicators with associated **support** and **confidence**  
- High-confidence rules capture strong textual signals of ADR

### **3. Machine Learning Baselines**
- Logistic Regression  
- Linear SVM  

### **4. Hybrid Model (Core Idea)**
A simple confidence-based rule:

> **If a high-confidence ADR rule fires → use rule prediction**  
> **Else → rely on the ML classifier**

This creates a lightweight interpretable “override layer” on top of ML.

---

## Evaluation Summary

Five models were evaluated on held-out test data:

| Model | Description | F1-score |
|-------|-------------|----------|
| **Rule-Based** | Apriori-mined lexical ADR indicators | 0.498797 |
| **Logistic Regression** | Logistic Regression on TF–IDF features | 0.724055 |
| **SVM** | Support Vector Machine on TF–IDF features | 0.721514 |
| **Hybrid (Rule + LR)** | Rules override LR when confidence ≥ 0.80 | 0.760694 |
| **Hybrid (Rule + SVM)** | Rules override SVM when confidence ≥ 0.80 | 0.752801 |


**Key observations:**
- High-confidence rules achieve strong precision for known ADR terms.  
- ML handles ambiguous or context-dependent phrases better than rules.  
- The hybrid model improves robustness, especially on text with clear ADR indicators.  

---

## Key Insights

- Symbolic rules and ML classifiers make complementary errors.  
- Rules give interpretability, ML gives generalization.  
- A simple confidence gate can improve consistency on edge cases.  
- Hybrid models provide a good starting point for interpretable biomedical NLP.
