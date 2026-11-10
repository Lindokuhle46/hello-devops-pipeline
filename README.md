# Hello devops pipeline

A small Flask API with automated tests, Docker, and a GitHub Actions CI pipeline.

## What it does
- `GET /` returns a greeting
- `GET /health` returns `{"status": "ok"}`

## The pipeline
On every push to `main`, GitHub Actions:
1. Checks out the code
2. Installs dependencies
3. Lints with ruff
4. Runs tests with pytest
5. Builds the Docker image

If any step fails, the pipeline stops and later steps are skipped.

## Run it locally
    docker compose up --build
    curl http://localhost:5000/health

## Proof the pipeline works
I broke the health check on purpose to confirm the pipeline catches mistakes. pytest failed, the Docker build was skipped, and after I fixed it the run went green again.

![Actions runs: green, red, green](Screenshot%202026-11-10%20020856.png)

![Failed pytest step](Screenshot%202026-11-10%20021048.png)

![Successful run](Screenshot%202026-11-10%20021253.png)

## Problems I hit and how I fixed them
- WSL on Windows would not accept my password, so I switched to GitHub Codespaces instead.
- Pasting multi-line commands into the terminal came out garbled, so I created the files in the editor instead.
- My first "break it on purpose" commit did nothing because the file was not saved, so git saw no changes.

## What I learned
As a second-year student new to CI/CD, this project taught me that "it works on my laptop" isn't good enough. A pipeline runs my code on a fresh machine every time I push, so mistakes get caught by a robot instead of by users. I proved it by breaking my health check on purpose: pytest failed, the Docker build was skipped, and a one-line fix turned the run green again. Next I want to add monitoring with Prometheus and Grafana.
