# ERP Ticket Analysis System

A comprehensive system for analyzing ERP support tickets, including data cleaning, classification, retrieval, and summarization components organized in multiple phases.

## Project Overview

This project implements a multi-phase pipeline for processing ERP helpdesk tickets:

1. **Phase 1**: Data ingestion, PII masking, and ticket classification
2. **Phase 2**: Retrieval with confidence-aware gating and reranking
3. **Phase 3**: Fine-tuned summarization with deterministic repair
4. **Phase 4**: Analysis and evaluation metrics

The system is designed to identify ticket categories, retrieve similar past tickets, and generate actionable, structured summaries.

## Setup Instructions

### Prerequisites

- Python 3.8 or higher
- Optional: CUDA-compatible GPU or Intel XPU for accelerated training

### Installation

1. Clone this repository
2. Create and activate a virtual environment (recommended):
   ```
   python -m venv venv
   # On Windows
   venv\Scripts\activate
   # On macOS/Linux
   source venv/bin/activate
   ```
3. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
4. Create necessary directories:
   ```
   mkdir -p data/raw data/processed artifacts outputs
   ```

## Running the Project

The system is implemented in the Jupyter notebook `erp_analysis.ipynb`.

### Data Requirements

Place these files in the `data/raw/` directory:
- `Copy of Tickets_cleaned.xlsx` - Main ticket data
- `Ticket history 3.xlsx` - Ticket history data

### Phase 1: Classification

This phase handles data loading, cleaning, PII masking, and training a RoBERTa classifier.

**Quick Start:**
- Run cells from notebook start to the "PHASE 2" marker
- **Note**: To skip recreating embeddings and pii cleanup, you can jump to the cell "Save and load embedded tickets for phase 1" to load preprocessed data directly to train the classifier 

**Outputs:**
- Cleaned tickets data
- FAISS index
- Trained classification model

### Phase 2: Retrieval System

This phase builds an enhanced retrieval system with confidence-aware gating.

**Quick Start:**
- Ensure pretrained saved models are available
- **Important**: Cell 3 must be run first to generate tickets with history
- Run cells between "PHASE 2" and "PHASE 3" markers

**Outputs:**
- Enhanced FAISS index with TEXT_FULL (including history)
- Baseline summaries for tickets

### Phase 3: Summary Generation

This phase implements a LoRA-adapted fine-tuned summarizer with a deterministic repair mechanism.

**Quick Start:**
- Models from Phase 2 can be loaded directly
- Refined tickets can be loaded to skip retraining
- Run cells between "PHASE 3" and "PHASE 4" markers

**Outputs:**
- LoRA adapter for the summarization model
- Enhanced summaries with consistent structure

### Phase 4: Evaluation

This phase analyzes the system performance with various metrics and visualizations.

**Quick Start:**
- Run cells from "PHASE 4" marker to the end

**Outputs:**
- Performance metrics
- Comparison visualizations
- Analysis reports

## System Architecture

The system follows a modular pipeline design:
1. **Data Processing**: PII masking, text normalization
2. **Classification**: RoBERTa-based category prediction
3. **Retrieval**: MPNet embeddings + FAISS + cross-encoder reranking
4. **Summarization**: LoRA-adapted T5 model + deterministic repair

## Folder Structure

```
Project Code/
├── artifacts/             # Saved models and intermediate outputs
├── data/
│   ├── raw/               # Raw input data files
│   └── processed/         # Processed data
├── outputs/               # Generated results and models
└── erp_analysis.ipynb     # Main notebook with all pipeline phases
```

## Use of AI Acknowledgement

Parts of this codebase were developed with the assistance of generative AI tools (e.g., ChatGPT and GitHub Copilot). These tools supported identification and resolution of errors, helped design modular and readable code, and contributed to maintaining clear project structure throughout implementation. All critical design decisions, algorithmic logic, and final code validation were carried out by the author.

