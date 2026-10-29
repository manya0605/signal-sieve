Signalsieve AIOps 🚀

AI-Powered Cross-Service Alert Correlation and Incident Intelligence

SignalSieve AIOps is an AIOps prototype designed to help IT and DevOps teams understand large volumes of system alerts. It correlates potentially related alerts across dependent services, ranks possible root causes, identifies repeated alert patterns, and recommends alert-tuning improvements for human review.

The goal is to help operators move from reviewing individual alerts to investigating related incidents through a single dashboard.

🎯 Problem Statement

Modern software systems generate alerts from many interconnected services. A single underlying problem can trigger multiple alerts, making it difficult for engineers to identify the primary issue and distinguish important incidents from repetitive alert patterns.

SignalSieve AIOps addresses this challenge through cross-service alert correlation, heuristic root-cause analysis, alert-pattern analysis, and human-reviewed tuning recommendations.

💡 Key Features

- Cross-Service Alert Correlation: Groups potentially related alerts using timestamps, service dependencies, and correlation logic.
- Root-Cause Analysis: Ranks potential causes using dependency coverage, direct alert evidence, and symptom keywords.
- Alert Noise Analysis: Identifies repeated alert patterns and unlabelled patterns that may require investigation.
- Tuning Recommendations: Suggests potential alert-rule improvements for operator review.
- Interactive Dashboard: Displays alert summaries, incident groups, root-cause candidates, noise analysis, and tuning recommendations.
- REST API: Exposes analysis functionality through FastAPI endpoints.
- Threshold-Based Alert Testing: Includes a separate test that generates 400 synthetic alerts and checks for three configured threshold breaches.
- Human-in-the-Loop Safety: Does not automatically suppress or disable alerts.

🏗️ System Architecture

Synthetic Alert Dataset (CSV)
             |
             v
Cross-Service Alert Correlation
             |
             v
     Candidate Incident Groups
             |
       +-----+------+
       |            |
       v            v
Root-Cause       Alert Noise
Analysis         Analysis
       |            |
       +-----+------+
             |
             v
Tuning Recommendations
             |
             v
       FastAPI Backend
             |
             v
    Streamlit Dashboard
             |
             v
     Human Review

🛠️ Technology Stack

Component| Technology
Programming language| Python
Data processing| Pandas, NumPy
Backend API| FastAPI
API server| Uvicorn
Dashboard| Streamlit
Visualization| Plotly
System monitoring support| psutil
Data storage in current prototype| CSV files
Development environment| Visual Studio Code

📁 Project Structure

SignalSieve_AIOps/
│
├── .venv/                         # Local Python environment (not committed)
├── requirements.txt               # Python dependencies
│
└── data/
    ├── app.py                     # Streamlit dashboard
    ├── api.py                     # FastAPI backend
    ├── realistic_alerts.csv       # Main synthetic alert dataset
    ├── demo_alerts.csv            # Demo dataset
    ├── cross_service_correlated_alerts.csv
    ├── root_cause_results.csv
    ├── noise_analysis_results.csv
    ├── tuning_recommendations.csv
    ├── cross_service_correlation.py
    ├── dependency_map.py
    ├── root_cause.py
    ├── root_cause_integration.py
    ├── noise_analyzer.py
    ├── tuning_recommender.py
    ├── create_realistic_data.py
    ├── data_engine.py
    ├── evaluate.py
    ├── evaluate_blind.py
    ├── alert_analyzer.py
    └── test_alerts.py

Generated CSV result files may be recreated when the analysis runs. Keep any required input CSV files in the project when packaging the application.

⚙️ Installation and Setup

Prerequisites

Install the following:

- Python 3.10 or a compatible version
- Visual Studio Code
- Git (optional, for cloning the repository)

1. Clone the repository

git clone <YOUR_GITHUB_REPOSITORY_URL>
cd SignalSage_AIOps

Alternatively, download and extract the project ZIP, then open the extracted folder in VS Code.

2. Create a virtual environment

python -m venv .venv

3. Activate the environment

.\.venv\Scripts\Activate.ps1

If PowerShell blocks activation, you can run the project's Python executable directly as shown in the commands below.

4. Install dependencies

python -m pip install --upgrade pip
pip install -r requirements.txt

If the project uses additional packages not listed in "requirements.txt", add those dependencies to the file before distributing the final version.

▶️ Running the Application

Run the backend and dashboard in separate VS Code terminals.

Terminal 1: Start the FastAPI backend

From the project root:

python -m uvicorn data.api:app --host 127.0.0.1 --port 8000

The API will be available locally at:

- Base endpoint: http://127.0.0.1:8000/
- Health check: http://127.0.0.1:8000/health
- Interactive API documentation: http://127.0.0.1:8000/docs

Keep this terminal open while using the backend.

Terminal 2: Start the Streamlit dashboard

From the project root:

python -m streamlit run data/app.py

Open the local dashboard URL displayed in the terminal. It is commonly:

http://localhost:8501

Note: If your dashboard entry file is located somewhere other than "data/app.py", use its actual path.

🔌 API Endpoints

Method| Endpoint| Purpose
GET| "/"| Project status and endpoint information
GET| "/health"| Backend health and dataset availability
GET| "/analyze"| Runs cross-service correlation and root-cause analysis
GET| "/noise"| Analyzes alert patterns for possible noise
GET| "/tuning"| Generates alert-tuning recommendations

You can test these endpoints through the interactive documentation at "http://127.0.0.1:8000/docs".

🧪 Testing

Run the 400-alert threshold test

From the project root:

python data/test_alerts.py

The recorded development test produced:

- Total synthetic alerts: 400
- Threshold breaches detected: 3
- Test result: Passed

This test validates the configured threshold-detection logic. It does not mean that the system correlates 400 alerts into three incidents.

Test the main analysis pipeline

1. Start the FastAPI backend.
2. Open "http://127.0.0.1:8000/docs".
3. Execute "GET /analyze".
4. Inspect the returned correlation groups and root-cause candidates.
5. Execute "GET /noise" to review alert-pattern analysis.
6. Execute "GET /tuning" to review the generated recommendations.
7. Open the Streamlit dashboard and verify the corresponding sections.

The recorded synthetic-data run processed 21 alerts into eight candidate groups. Results may vary if the input dataset or correlation logic changes.

📊 Evaluation and Results

The current prototype demonstrates:

- Correlation of alerts across multiple services.
- Candidate incident grouping based on time and dependency information.
- Heuristic ranking of possible root causes.
- Identification of repeated patterns for further investigation.
- Generation of human-reviewed tuning recommendations.
- Dashboard access to analysis results through a backend API.

These are prototype results using synthetic data. They should not be interpreted as production accuracy, real-world detection rates, or a measured reduction in operational alert volume.

🔐 Safety and Limitations

- The main dashboard uses a small synthetic dataset rather than a live monitoring feed.
- Root-cause scores are heuristic evidence scores, not trained-model probabilities or proof of causation.
- Repeated or unlabelled alert patterns are investigation candidates, not automatically confirmed noise.
- Alert tuning recommendations require human review.
- The prototype does not automatically suppress alerts or remediate services.
- A public hosted deployment is separate from local execution and must be configured and tested independently.

🚀 Future Enhancements

- Integrate live alerts from monitoring platforms.
- Improve correlation with configurable service dependency maps and time windows.
- Evaluate grouping precision and recall on larger, independently labelled datasets.
- Add historical incident storage and trend analysis.
- Introduce authenticated users and role-based access control.
- Add alert feedback to help operators evaluate recommendation quality.
- Deploy the dashboard and API to a public hosting platform.
- Add monitoring, structured logging, and automated tests.

👥 Team

Project: SignalSieve AIOps

Team Name: Spectrum

Team ID: HM3-P-00566-02

Team Members:

- Monika V
- Manya M V
- Lalitha M N
- Prathiksha Sanjeev

SignalSieve AIOps — From alert overload to incident intelligence.
