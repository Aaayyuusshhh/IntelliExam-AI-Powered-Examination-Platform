# 📝 IntelliExam: AI-Powered Examination Platform  

End-to-end AI grading system for automated evaluation of student submissions.  
Processes scanned answer sheets using OCR, applies semantic similarity for content scoring, and enables LLM-driven evaluation workflows for scalable assessment.

---

## 🚀 Overview  

- Architected an **AI-based grading system** processing 500+ real student submissions  
- Integrated **Tesseract OCR** for extracting text from scanned answer sheets  
- Implemented **BERT-based semantic similarity** for automated content evaluation  
- Built **REST APIs for bulk grading workflows**, reducing manual effort by ~70%  
- Developed **LLM-driven question generation and evaluation pipelines**  
- Containerized the system using **Docker** for reproducible deployment  

## ✨ Key Features

- **🎯 PPT-Focused Processing**: Direct PowerPoint analysis with accurate slide counting and content extraction using Docling
- **🔍 Dynamic Content Extraction**: No hardcoded values - automatically adapts to any PPT structure
- **🖼️ Smart Image Extraction & OCR**: Extracts meaningful images and uses pytesseract to extract text from technical diagrams
- **🧠 AI-Powered Evaluation**: Multi-model ensemble evaluation using IBM Granite and Meta LLaMA models
- **📊 Technical Architecture Analysis**: Specialized OCR processing for architecture diagrams with automatic keyword detection
- **👥 Team Member Detection**: Automatically identifies team member names from presentation content
- **📁 Clean Directory Structure**: Creates organized output with parquet files, images, and evaluation results
- **🎨 Web Interface**: Beautiful Gradio-based frontend for easy file upload and results visualization
- **⚙️ IBM Data Prep Kit Integration**: Advanced text preprocessing with deduplication, quality filtering, and chunking

---

## 🔧 Tech Stack  
**Core Language**  
- Python  

**NLP & Modeling**  
- Transformers (BERT) for semantic similarity and content evaluation  
- Scikit-learn for supporting ML pipelines and evaluation  

**OCR & Document Processing**  
- Tesseract OCR for text extraction from scanned submissions  
- python-pptx and document processing utilities for structured content parsing  

**LLM & Evaluation**  
- IBM watsonx.ai / OpenAI APIs for automated grading and question generation  
- Prompt engineering for controlled and consistent evaluation  

**Backend & APIs**  
- Flask for REST-based bulk grading and pipeline orchestration  

**Data Processing**  
- Pandas and NumPy for preprocessing and data handling  

**Deployment & Infrastructure**  
- Docker for containerized deployment and reproducibility
---

## 🚀 Quick Start

### Option 1: Web Interface (Recommended)
```bash
# Install dependencies
pip install -r requirements.txt

# Launch the web interface
python frontend.py
```
Then open your browser and navigate to the displayed URL (usually http://localhost:7860)

### Option 2: Command Line
```bash
# Process PowerPoint files with default settings
python ai_evaluator_pipeline.py

# Process specific input/output directories
python ai_evaluator_pipeline.py --input input_submissions --output pipeline_output

# Run evaluation on processed files
python report_generator.py --input_dir pipeline_output --out_prefix evaluation_results

# View help
python ai_evaluator_pipeline.py --help
```

## 🎯 Evaluation Criteria

The system evaluates presentations across multiple dimensions:

- **Problem Statement**: Uniqueness, completeness, impact, and ethical considerations
- **Proposed Solution**: Technical feasibility, innovation, and implementation details  
- **Technical Architecture**: System design, scalability, and technical depth (includes OCR analysis of diagrams)

Each section is scored by multiple AI models and aggregated for consistent results.

## 📁 Project Structure

```
AI_evaluator/
├── ai_evaluator_pipeline.py # Main orchestration pipeline (OCR + evaluation)
├── report_generator.py # Multi-model evaluation engine (LLM-based scoring)
├── frontend.py # Gradio-based web interface
├── image.py # OCR extraction from images (pytesseract)
├── config_template.json # Configuration template
├── requirements.txt # Dependencies
├── Dockerfile # Containerized deployment setup
│
├── doc_processing/ # Document processing layer
│ ├── main_processor.py # Core PPT/document processor (Docling integration)
│ ├── config/settings.py # Configuration management
│ └── utils/ # Processing utilities
│ ├── enhanced_content_extractor.py # Dynamic content extraction
│ ├── embedding_generator.py # Embedding + semantic mapping
│ ├── vector_storage.py # Vector storage (ChromaDB)
│ ├── dpk_preprocessor.py # IBM Data Prep Kit preprocessing
│ └── file_handler.py # File handling utilities
│
├── data_processing/ # Data processing framework (IBM Data Prep Kit)
│ ├── transform/ # Transform modules
│ ├── utils/ # Utilities and configs
│ ├── data_access/ # Data access abstractions
│ └── runtime/ # Execution framework
│
├── input_submissions/ # Input PPT / answer sheets
├── pipeline_output/ # Processed outputs (parquet + images)
├── vector_db/ # Vector database storage
├── analysis_output/ # Evaluation results and reports
│
└── examples/
└── MindEase.pptx # Sample input file
```
## 🛠️ Dependencies

```bash
# Core document processing
python-pptx>=0.6.21
docling>=2.3.1

# OCR & image processing
pytesseract
Pillow

# Data processing and storage  
pandas>=1.5.0
pyarrow>=10.0.0
numpy>=1.21.0

# NLP & embeddings
sentence-transformers
transformers
torch

# Vector storage
chromadb

# LLM integration
openai
google-generativeai   # or IBM watsonx.ai SDK (if used)

# Backend / APIs
flask

# Deployment & environment
docker

# Utilities
pathlib
logging
json
```

## 🏗️ System Architecture

```
┌─────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│ PPT Files       │───▶│ Direct PPT         │───▶│ IBM Data Prep Kit   │
│ (.ppt/.pptx)    │    │ Processing          │    │ (Dedup & Quality)   │
└─────────────────┘    │ (python-pptx +      │    └─────────────────────┘
                       │  Docling)           │              │
                       └─────────────────────┘              ▼
                                 │              ┌─────────────────────┐
                                 ▼              │ Images Folder       │
                       ┌─────────────────────┐  │ (Filtered)          │
                       │ PPT Folder          │  │ slide_XX_XX.jpg     │
                       │ name.parquet        │  └─────────────────────┘
                       │ (Slide Data)        │              │
                       └─────────────────────┘              │
                                 ▲                          │
                                 |                          │
                                 |                          ▼
                                 |                ┌─────────────────────┐
                                 |                │ OCR (image.py)      │
                                 |                │ pytesseract         │
                                 |                │ → image_extracted   │
                                 |                │   text              │
                                 |                └─────────────────────┘
                                 |                          │
                                 |                          │
                                 |                          │
                                 |                          ▼
┌─────────────────────┐          |                ┌─────────────────────┐    ┌─────────────────────┐
│ Enhanced Content    │◀─────────┘                │ Evaluation          │───▶│ Results            │
│ Extractor (Dynamic) │                           │ report_generator.py │    │ (CSV/Parquet)       │
└─────────────────────┘                           └─────────────────────┘    │ results/            │
          ▲                                                 ▲                │ evaluation*.csv     │
          │                                                 │                └─────────────────────┘
          │                                                 │ run
┌─────────────────────┐         ┌─────────────────────┐     │
│ Embedding Generator │◀───────▶│ In-Memory Vector   │     │
│ (Optional)          │         │ Store (Optional)    │     │
└─────────────────────┘         └─────────────────────┘     │
          ▲                                                 │
          │                                                 │
          │                                       ┌─────────────────────┐    ┌─────────────────────┐
          │                                       │ Web UI (Gradio)     │───▶│ Plotly Tables &    │
          └───────────────────────────────────────│ frontend.py         │    │ Charts              │
                                                  └─────────────────────┘    └─────────────────────┘
```

## 📊 Sample Output

After processing `COSMIC_CODERS.pptx`, you'll get:

```
pipeline_output/
└── COSMIC_CODERS/
    ├── COSMIC_CODERS.parquet    # 12 slides with complete content
    └── images/                  # Only meaningful images
        ├── slide_05_diagram_01.png
        └── slide_06_chart_01.jpg
```

**Parquet Content**: Each row contains slide_number, title, content, total_pages, images_present, extracted_images, image_count, and processing_timestamp.

---

**AI Idea Evaluator** - Simplified PowerPoint processing pipeline with dynamic extraction and clean output structure.

*Built with ❤️ using python-pptx, Docling, and intelligent content filtering*
