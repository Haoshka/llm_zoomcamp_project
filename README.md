# LLM Zoomcamp Project

This repository contains my project work for **LLM Zoomcamp**. It demonstrates a full-stack LLM-based application with a backend API, database, Streamlit frontend, and observability via Grafana, all runnable locally or with Docker.

---

## Project Overview

The project aims to create a web application that is empowered by LLM and answers questions to the documents used in freight forwarding industry. 

- Building an LLM-powered backend service
- Using PostgreSQL as the application database
- Running services locally and in Docker
- Visualizing metrics with Grafana
- Providing a Streamlit-based UI

🎥 **Demo Video:**  
https://youtu.be/YAiM_a28oPY

---

## Repository Structure (High-Level)

```text
llm_zoomcamp_project/
├── app/                 # Backend application code
│   ├── app.py           # Main backend service
│   ├── db_prep.py       # Database initialization script
│   └── data/            # CSV and other data files
├── grafana/             # Grafana setup and initialization
├── streamlit_app.py     # Streamlit frontend
├── Dockerfile           # Application Docker image
├── docker-compose.yml   # Local service orchestration
├── Pipfile / Pipfile.lock
└── README.md
```

---

## Prerequisites

Make sure you have the following installed:

- Python 3.10+
- `pipenv`
- Docker & Docker Compose
- PostgreSQL (local or containerized)

You will also need a **GROQ API key** available as an environment variable.

```bash
export GROQ_API_KEY=your_api_key_here
```

---

## Environment Variables

Common environment variables used across the project:

- `GROQ_API_KEY` – API key for the LLM provider
- `POSTGRES_HOST` – PostgreSQL hostname (usually `localhost`)
- `DATA_PATH` – Path to the CSV data file
- `BACKEND_URL` – URL of the backend service (for Streamlit)

---

## Running with Docker

### Useful Docker Command

Run the backend application container:

```bash
docker run -it --rm \
    --network="llm_zoomcamp_default" \
    --env-file=".env" \
    -e GROQ_API_KEY=${GROQ_API_KEY} \
    -e DATA_PATH="data/data.csv" \
    -p 5000:5000 \
    app
```

### Inspect Container Files

```bash
docker run -it --rm app bash
ls -R /app
python - <<'EOF'
from pathlib import Path
print(list(Path("/app/data").glob("*")))
EOF
```

### Check CSV File Inside a Running Container

```bash
docker exec -it <container id> bash ls data/
```

---

## Local Development (Virtual Environment)

### Initialize the Database

```bash
pipenv shell

cd app

export POSTGRES_HOST=localhost
python db_prep.py
```

### Run the Backend Application

```bash
pipenv shell

cd app

export POSTGRES_HOST=localhost
python app.py
```

The backend should be available at:

```
http://localhost:5000
```

---

## Streamlit Frontend

Run the Streamlit application locally:

```bash
export POSTGRES_HOST=localhost
export BACKEND_URL="http://localhost:5000"
streamlit run streamlit_app.py
```

Once started, Streamlit will provide a local URL in the terminal.

---

## Grafana

Grafana is used for monitoring and visualization.

### Initialize Grafana

```bash
pipenv shell
cd grafana

# Make sure the POSTGRES_HOST variable is not overwritten
env | grep POSTGRES_HOST
python init.py
```

After initialization, Grafana should be available according to your Docker or local setup.

---

## Notes

- This project is primarily for learning and experimentation.
- Commands may evolve as the project grows.
- Contributions, issues, and suggestions are welcome.

---

## License

This project is provided for educational purposes. Add a license file if you plan to reuse or distribute this code publicly.

