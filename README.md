# 🤖 Virtual Software Development Team (AI Agent Swarm)

This project simulates a professional software development lifecycle using a Multi-Agent System (MAS). It employs a **Hierarchical State Graph** architecture to coordinate agents (Product Manager, Architect, Developer, QA) who work together to plan, code, test, and debug software tasks autonomously.

## 🏗 Architecture

The system uses **LangGraph** to manage state and cyclical workflows (loops), ensuring that code is not "finished" until it passes tests.

- **Orchestration:** LangGraph (Python)
- **Intelligence:** Claude 3.5 Sonnet (via Anthropic API) or GPT-4o
- **Execution Sandbox:** E2B Code Interpreter (Secure Cloud Micro-VMs)
- **Tooling:** GitHub API, Linear/Jira API (Optional)

### Workflow Visualization
*(Paste the Mermaid.js code from the previous conversation here)*


## 📋 Prerequisites

Before running the application, ensure you have the following installed on your local machine:

### 1. System Requirements
- **OS:** macOS, Linux, or Windows (WSL2 recommended)
- **Python:** Version **3.10** or **3.11** (Recommended for stability with LangChain/LangGraph)
- **Git:** Latest version
- **direnv:** Latest version

### 2. External Accounts & API Keys
You will need API keys for the services that power the agents.

| Service | Purpose | Cost |
| :--- | :--- | :--- |
| **[Anthropic API](https://console.anthropic.com/)** | Provides the intelligence. | Paid |
| **[Open AI API](https://openai.com/api/)** | Provides the intelligence (Open AI API). | Paid | 
| **[Gemini API](https://ai.google.dev/gemini-api/docs)** | Provides the intelligence (Gemini API) | Paid |
| **[Perplexity API](https://www.perplexity.ai/api-platform)** | Provides intelligence (Perplexity API) | Paid | 
| **[E2B](https://e2b.dev/)** | Provides the secure cloud sandbox where agents run code. | Free Tier Available |
| **[LangSmith](https://smith.langchain.com/)** | (Optional but recommended) For tracing and debugging agent loops. | Free Tier Available |
| **[GitHub Token](https://github.com/settings/tokens)** | Allows agents to clone repos and open Pull Requests. | Free |
| **[n8n](https://github.com/n8n-io/n8n)** | (_Optional_) Provides an infrastructure to run a DAG(Directed Acyclic Graph), which triggers the whole multi-agent workflow to run iteratively | Free | 

## 📦 Dependencies
* The state machine library for building cyclic agent workflows
  * `langgraph`
* The base framework for LLM interactions
  * `langchain`
  * `langchain-core`
  * `langchain-community`
* Model Providers(Open AI, Anthropic, Gemini, and Perplexity)
  * `langchain-openai`
  * `langchain-anthropic`
  * `langchain-google-genai`
  * `langchain-perplexity`
* The SDK for the secure cloud sandbox to run code
  * `e2b-code-interpreter`
* For defining strict data schemas (Agent inputs/outputs)
  * `pydantic`
* For pretty-printing logs in the terminal
  * `termcolor`


## ⚙️ Installation

Follow these steps to set up the development environment.

### 1. Clone the Repository
```bash
git clone [https://github.com/yourusername/virtual-software-team.git](https://github.com/yourusername/virtual-software-team.git)
cd virtual-software-team
```

### 2. Create a Virtual Environment

It is highly recommended to isolate dependencies.

```bash
# MacOS/Linux
python3.11 -m venv venv
source venv/bin/activate

# or
pyenv virtualenv <PYTHON_VERSION> <VIRTUAL_ENV_NAME>
pyenv local <VIRTUAL_ENV_NAME>

# Windows (PowerShell)
python -m venv venv
.\venv\Scripts\activate
```

### 3. Install Dependencies

Create a requirements.txt file with the content below, then install it.

```bash
pip install -r requirements.txt
```


## Configuration

### 1. Create a `.envrc` file in the root directory:

```bash
touch .envrc
```

### 2. Add your API keys to the `.envrc` file. Note: Do Not commit this file to version control. By default, it won't be committed as .gitignore file knows files begin with `.env*` to be ignored.

```shell
# .envrc file

# The Brains (Choose one or both)
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...

# The Hands (Sandboxed Environment)
E2B_API_KEY=e2b_...

# The Tools
GITHUB_ACCESS_TOKEN=ghp_...

# Observability (Optional)
LANGCHAIN_TRACING_V2=true
LANGCHAIN_ENDPOINT="[https://api.smith.langchain.com](https://api.smith.langchain.com)"
LANGCHAIN_API_KEY=lsv2_...
LANGCHAIN_PROJECT="virtual-software-team"
```


# 🚀 Usage

The entry point of the application is main.py.

## Running a specific task

You can run the swarm by passing a natural language instruction. The "PM Agent" will parse this into actionable steps.

```Bash
python main.py --task "Create a snake game in Python using pygame and write tests for it."
```

## Running in Interactive Mode

To chat with the "Engineering Manager" agent who will dispatch tasks:

```Bash
python main.py --interactive
```


# 📂 Project Structure

```Plaintext
virtual-software-team/
├── agents/                 # Agent Definitions
│   ├── __init__.py
│   ├── product_manager.py  # Requirements gathering
│   ├── tech_lead.py        # Architecture & File selection
│   ├── developer.py        # Coding (Calls E2B)
│   └── qa_engineer.py      # Testing (Calls E2B)
├── tools/                  # Custom Tools
│   ├── sandbox.py          # E2B Setup & Wrapper
│   └── github_ops.py       # Git Clone/Push logic
├── graph/                  # LangGraph Logic
│   └── workflow.py         # The Node & Edge definitions
├── .env                    # Secrets (Ignored by Git)
├── main.py                 # Entry point
├── requirements.txt        # Dependencies
└── README.md               # Documentation
```

# ⚠️ Safety Notice
This application executes code generated by AI. While E2B provides a secure, sandboxed environment preventing damage to your local machine, always review the code and the Pull Requests generated by the agents before merging them into production repositories.