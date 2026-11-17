# FortiX — Graph-Based UPI Fraud Detection

<p align="center">
  <img src="https://img.shields.io/badge/FortiX-Fraud%20Detection-7c3aed?style=for-the-badge&logo=github&logoColor=white" alt="FortiX"/>
  <img src="https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/Graph%20Learning-GNN-0ea5e9?style=for-the-badge" alt="GNN"/>
  <img src="https://img.shields.io/badge/Dashboard-Streamlit-ff4b4b?style=for-the-badge&logo=streamlit&logoColor=white" alt="Streamlit"/>
</p>

<p align="center">
  <b>Graph Intelligence • Transaction Analytics • Fraud Investigation</b><br/>
  A graph-based platform for exploring suspicious UPI transaction behaviour using node and edge learning, interactive network analysis, and an analyst-focused dashboard.
</p>

<p align="center">
  <a href="https://github.com/SAITEJA8326/FortiX">Repository</a> •
  <a href="https://github.com/SAITEJA8326/FortiX/tree/main/frontend">Dashboard</a> •
  <a href="https://github.com/SAITEJA8326/FortiX/tree/main/backend">API</a> •
  <a href="https://github.com/SAITEJA8326/FortiX/tree/main/models">GNN Models</a>
</p>

---

## Architecture

<p align="center">
  <img src="docs/fortix-architecture.png" alt="FortiX system architecture" width="100%"/>
</p>

The architecture brings together the GNN training pipeline, graph analytics, analyst dashboard, documented investigation capabilities, and the transaction-facing API.

## Overview

UPI transactions form a network of accounts and payments. Looking at a transaction in isolation can hide relationships such as multiple accounts transferring funds to a common recipient or money moving through circular paths.

**FortiX** models these relationships as a graph:

- **Nodes** represent UPI accounts or VPAs.
- **Edges** represent transactions between accounts.
- **Node classification** estimates whether an account is suspicious.
- **Edge classification** estimates whether a transaction is fraudulent.
- **Graph analytics** help analysts inspect transaction relationships and patterns.

The repository contains a GNN training workflow, data-generation and preprocessing scripts, a Streamlit dashboard, and a lightweight HTTP API demo.

## Core Capabilities

| Area | What it provides |
|---|---|
| Graph-based modelling | Directed transaction graph with account nodes and payment edges |
| Node and edge learning | GAT-based node classifier and GraphSAGE-based edge classifier |
| Feature engineering | Account activity statistics, in/out degree, transaction amount, and timing features |
| Synthetic data | Script to generate transactions with injected fraud patterns |
| Graph investigation | Network visualisation and graph-oriented fraud analysis |
| Analyst dashboard | Streamlit interface with analytics, alerts, investigation views, and model-performance visualisations |
| API demonstration | Login, JWT-protected transaction submission, risk lookup, and alert explanation routes |
| Guardian Mode | Dashboard capability for stricter handling of potentially vulnerable users |

## Technology Stack

<p align="center">
  <img src="https://skillicons.dev/icons?i=python,pytorch,flask,streamlit,git,github" alt="Technology icons"/>
</p>

| Component | Technology |
|---|---|
| Language | Python |
| GNN / deep learning | PyTorch, PyTorch Geometric |
| Graph processing | NetworkX |
| Data processing | Pandas, NumPy |
| Machine learning utilities | scikit-learn |
| Dashboard | Streamlit |
| Visualisation | Plotly |
| API authentication | PyJWT |
| API server | Python standard-library HTTP server |

## Repository Structure

```text
FortiX/
├── backend/
│   ├── auth.py                 # JWT creation and verification
│   ├── fraud.py                # API demo risk scoring and explanations
│   ├── server.py               # HTTP API routes
│   ├── storage.py              # In-memory transaction storage
│   └── requirements.txt
│
├── data/                       # Input and generated datasets
├── docs/
│   └── fortix-architecture.png # System architecture diagram
├── frontend/
│   ├── app.py                  # Streamlit dashboard
│   ├── components.py           # Dashboard components and charts
│   ├── utils.py                # Analytics and fraud-pattern utilities
│   ├── requirements.txt
│   └── README.md
│
├── lib/                        # Frontend supporting libraries
├── models/
│   └── gnn_fraud_detection.py  # NodeGNN and EdgeGNN definitions
│
├── data_inspection.py          # Inspect transaction dataset
├── data_loader.py              # Kaggle dataset download helper
├── feature_extraction.py       # Synthetic transaction generation
├── preprocessing.py            # Node/edge preprocessing
├── train.py                    # GNN training and evaluation
└── README.md
```

## How It Works

1. **Data acquisition / generation** — use the dataset loader or generate a synthetic transaction dataset.
2. **Preprocessing** — clean and transform transaction records into node and edge tables, deriving account-level and transaction-level features.
3. **Graph construction** — create a directed graph from sender and receiver identifiers.
4. **Model training** — train separate GNN models for node and edge classification.
5. **Graph analysis** — inspect transaction relationships and suspicious structures.
6. **Dashboard review** — use the Streamlit interface to explore analytics, alerts, networks, and investigation views.
7. **API demonstration** — submit transactions to the HTTP API and retrieve stored transaction risk information.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/SAITEJA8326/FortiX.git
cd FortiX
```

### 2. Create a virtual environment

**Windows (PowerShell)**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install project dependencies

The dashboard and API have separate requirements files:

```bash
pip install -r frontend/requirements.txt
pip install -r backend/requirements.txt
```

The GNN training scripts also use PyTorch, PyTorch Geometric, NetworkX, Pandas, NumPy, and scikit-learn. Install a PyTorch / PyTorch Geometric combination compatible with your Python version and hardware by following their official installation instructions.

## Run the Dashboard

From the repository root:

```bash
pip install -r frontend/requirements.txt
streamlit run frontend/app.py
```

Streamlit prints a local URL (typically `http://localhost:8501`) to open in your browser.

The dashboard includes monitoring and analytics views, fraud-pattern visualisations, network exploration, investigation screens, and Guardian Mode controls. Some dashboard content is generated from sample data within the application.

## Run the API Demo

Open a second terminal from the repository root:

```bash
pip install -r backend/requirements.txt
cd backend
python server.py
```

The API listens on port `8000`.

### Available routes

| Method | Route | Purpose |
|---|---|---|
| POST | `/auth/login` | Authenticate and receive a JWT |
| POST | `/transaction/submit` | Submit a transaction (JWT required) |
| GET | `/transaction/risk/{transaction_id}` | Retrieve the stored transaction risk (JWT required) |
| GET | `/alerts/{transaction_id}` | Retrieve an explanation for a transaction alert (JWT required) |

For protected routes, send the token in the request header:

```http
Authorization: Bearer <your-jwt-token>
```

> **Demo security note:** The current API uses demonstration credentials, a hard-coded JWT secret, and in-memory storage. Its `fraud.py` scorer currently generates a random demo risk score; it is not wired to the trained GNN weights. Do not expose this API publicly or use it for real payment decisions without replacing these components with production-grade authentication, persistent storage, secure secret management, and an integrated, validated model-serving pipeline.

## Data and GNN Training

### Generate synthetic transactions

From the repository root:

```bash
python feature_extraction.py
```

This writes `transactions_synthetic.csv` in the current working directory. The generator injects synthetic money-muling and fan-in/fan-out patterns alongside normal transactions.

### Preprocess the data

```bash
python preprocessing.py
```

The preprocessing script expects `transactions_synthetic.csv` in the repository root and writes:

- `data/processed_nodes_synthetic.csv`
- `data/processed_edges_synthetic.csv`

Ensure the `data/` directory exists before running the script.

### Train the GNN models

```bash
python train.py
```

The training script reads the two processed CSV files, builds a directed NetworkX graph, derives node and edge features, creates train/test masks, trains the node and edge classifiers, evaluates them, and saves model weights as implemented in the script.

> The training workflow and the API demonstration are separate paths in the current codebase. Training the GNN does not automatically change the API demo scorer.

## Fraud Patterns and Investigation

The dashboard and graph utilities include investigation views for patterns such as:

- **Star-shaped activity** — many accounts transferring funds to a common account.
- **Circular flows** — funds moving through a cycle of accounts.
- **High-value anomalies** — transactions with unusually large amounts.
- **Account / VPA anomalies** — suspicious account behaviour and potential VPA similarity or spoofing signals in the GNN module.

These patterns are investigation signals, not proof of fraud. Their outputs should be reviewed with transaction context and appropriate validation.

## Current Implementation Notes

- The dashboard is a Streamlit application and includes internally generated sample data for several views.
- The GNN module defines separate node and edge models and graph feature utilities.
- The root training pipeline uses processed synthetic node and edge CSV files.
- The API's current fraud prediction function is a random demo scorer and is not a production model-serving endpoint.
- API transaction storage is in memory and is cleared when the server process restarts.
- The API currently uses Python's built-in HTTP server rather than Flask.

## Roadmap

- [ ] Connect trained GNN checkpoints to a dedicated inference service.
- [ ] Replace demo credentials and hard-coded secrets with secure configuration.
- [ ] Add persistent transaction storage.
- [ ] Add automated tests for preprocessing, model inference, and API routes.
- [ ] Add reproducible experiment configuration and model evaluation reports.
- [ ] Document the end-to-end deployment workflow.

## Contributing

Contributions, suggestions, and issue reports are welcome.

1. Fork the repository.
2. Create a focused feature branch.
3. Keep changes scoped and documented.
4. Run relevant checks before opening a pull request.
5. Include a clear summary and supporting screenshots or logs when applicable.

## License

No license file is currently included in the repository. Unless a license is added, standard copyright restrictions apply.

---

<p align="center">
  <b>FortiX</b><br/>
  Graph intelligence for safer digital payments.
  <br/><br/>
  <a href="https://github.com/SAITEJA8326/FortiX">View on GitHub</a>
</p>
