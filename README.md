# 🧠 InsightGraph: The Gemini-Powered Multi-Agent Researcher

## 🚀 Project Overview

This project is a **Multi-Agent System** designed to automate the analysis of dense scientific papers and technical documents. It uses a single, self-contained Jupyter Notebook to employ a team of specialized AI agents. These agents work sequentially to ingest data, memorize context, summarize findings, and programmatically generate knowledge graphs for visualization.

Unlike standard chatbots, this system uses **active tooling**. The agents write and execute their own Python code to create visualizations on the fly.

## ✨ Key Features

This project implements several advanced agentic concepts:

### 1. 🤖 Multi-Agent Architecture
The system uses a **Sequential Agent Workflow** with two distinct roles:
* **`Curie` (Senior Researcher):** Specialized in data ingestion, context compaction, and high-level summarization.
* **`DaVinci` (Data Visualizer):** Specialized in translating textual insights into executable Python code (using `NetworkX` and `Matplotlib`) for visualization.

### 2. 🧠 Long-Term Memory & RAG
* **Memory Bank:** Implements a persistent **Vector Store** using `FAISS` and `SentenceTransformers` directly within the notebook session.
* **Context Engineering:** The system chunks PDF data, embeds it, and allows agents to query the memory bank for specific details (RAG).

### 3. 🛠️ Custom Tools & Code Execution
* **PDF Ingestion Tool:** A custom tool using `pdfplumber` to parse, clean, and extract text from PDF documents.
* **Dynamic Code Execution:** The `VisualizerAgent` writes raw Python code, which is safely extracted using regex and executed by the system to render knowledge graphs immediately in the notebook output.

### 4. 👁️ Observability & Resilience
* **Agent Logger:** Custom color-coded logging provides real-time tracing of agent thoughts, tool inputs, and system status in the cell outputs.
* **Production-Grade Retry Logic:** Implements **Exponential Backoff** to handle API rate limits gracefully, ensuring recovery from `ResourceExhausted` errors.
* **Safe Mode Orchestrator:** Intelligent delays are built in between agent hand-offs to ensure stability on the Gemini Free Tier.

## 🏗️ System Architecture

```mermaid
graph TD
    User[User Input (PDF)] -->|Step 1| ResAgent[🕵️ Research Agent]
    
    subgraph "Memory System"
        ResAgent -->|Ingest| VectorDB[(FAISS Memory Bank)]
        VectorDB -->|Query Context| ResAgent
    end
    
    subgraph "Analysis Phase"
        ResAgent -->|Summary & Entities| VisAgent[🎨 Visualizer Agent]
    end
    
    subgraph "Execution Phase"
        VisAgent -->|Generates Python Code| ExecTool[⚙️ Code Executor]
        ExecTool -->|Renders| Output[📊 Knowledge Graph]
    end
````

## 💻 Setup & Usage

This project is designed to be "Plug and Play" within a single Jupyter Notebook.

### Prerequisites

  * A Google Gemini API Key.
  * An environment that runs Jupyter Notebooks (e.g., Google Colab, Kaggle, or local JupyterLab).

### Instructions

1.  **Open the Notebook** in your preferred environment.
2.  **Configure API Key:**
      * Create a `.env` file in the same directory with `GEMINI_API_KEY=your_key`, OR
      * Directly paste your key into the `API_KEY` variable in the configuration cell.
3.  **Run All Cells:**
      * The first cell will automatically install all necessary dependencies (`google-generativeai`, `faiss-cpu`, `pdfplumber`, `sentence-transformers`, `matplotlib`, `networkx`, `python-dotenv`, `colorama`).
      * Subsequent cells will initialize the Agents, Memory Bank, and Tool Box.
4.  **Watch the Magic:** The final cell executes the `main_system()` function. By default, it will download and analyze the Bitcoin Whitepaper (`bitcoin.pdf`) if a PDF is not provided. You will see color-coded logs followed by the generation of a Knowledge Graph visualization.
