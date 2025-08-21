# oai-debug-rag

Automated pipeline for generating `debug.yaml` entries from OAI gNB logs and configuration files.  
This project compares **None-RAG** (baseline) and **RAG-enhanced** approaches, where RAG integrates 3GPP/O-RAN specifications to improve debugging accuracy.

---

## ✨ Features

- **Config/Log Diff Analysis**  
  Extracts differences between success/failure `.conf` files and parses corresponding logs.

- **Error Summarization**  
  Uses an LLM to generate structured summaries containing:
  - Configuration parameter differences
  - Representative error log lines
  - Technical cause explanations
  - Semantic query variants for retrieval

- **RAG Integration**  
  - Retrieves top-k specification snippets (3GPP, O-RAN, OAI notes) from a Chroma vector database.  
  - Injects these contexts into the LLM prompt to enhance reasoning.

- **Dual YAML Generation**  
  - **None-RAG**: Generates `debug.yaml` only from diff + logs.  
  - **RAG**: Generates `debug.yaml` with additional spec snippets.  
  - Both outputs are saved for comparison.

- **Structured Output**  
  Each case produces:
  - `none_rag_debug_{index}.yaml`
  - `rag_debug_{index}.yaml`
  - Top-k retrieval records for traceability

---
## 📂 Project Structure
```bash=
📂 Project Root
├── 📓 auto_gen_debug_yaml.ipynb   # Main notebook pipeline
├── 📂 none_rag_debug_yaml/        # Outputs (baseline YAML)
├── 📂 rag_debug_yaml/             # Outputs (RAG-enhanced YAML)
├── 📂 retrieval_results/          # Top-k retrieved context from specs
├── 📂 baseline_conf/              # Reference configs
├── 📂 fail_conf/                  # Modified (failure) configs
└── 📂 log/                        # CU/DU logs
```

## 🚀 Workflow

1. **Prepare Inputs**
   - Place baseline and fail `.conf` files in respective folders
   - Place CU/DU logs in `log/`

2. **Run Notebook**
   - Execute `auto_gen_debug_yaml.ipynb`
   - LLM will:
     - Generate summary
     - Query vector DB for relevant 3GPP/O-RAN snippets
     - Produce both None-RAG and RAG YAMLs

3. **Check Outputs**
   - Compare generated YAMLs under `none_rag_debug_yaml/` vs `rag_debug_yaml/`
   - Review top-k retrieval logs

---

## 🔧 Requirements

- Python 3.9+
- [ChromaDB](https://docs.trychroma.com/)
- [NVIDIA API](https://build.nvidia.com/)
- [Google API](https://ai.google.dev/)

