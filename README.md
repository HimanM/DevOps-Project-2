# DevOps Project

A complete DevOps project with Next.js Frontend, Python Backend, Docker, Jenkins CI/CD, and Prometheus/Grafana Monitoring.

## Tech Stack
- **Frontend**: Next.js, Tailwind CSS, Shadcn UI
- **Backend**: Python (Flask)
- **Database/Infra**: Docker, Docker Compose
- **CI/CD**: Jenkins
- **Monitoring**: Prometheus, Grafana

## Project Structure
- `/frontend`: Next.js application
- `/backend`: Python Flask application
- `/monitoring`: Prometheus and Grafana config
- `Jenkinsfile*`: CI/CD pipelines
- `docker-compose.yml`: Local development setup

## Getting Started

### Prerequisites
- Docker & Docker Compose
- Node.js & npm
- Python 3.9+

### Local Development
1.  **Clone the repo**:
    ```bash
    git clone <repo_url>
    cd <repo_name>
    ```

2.  **Run with Docker Compose**:
    ```bash
    docker-compose up --build
    ```
    - Frontend: `http://localhost:3000`
    - Backend API: `http://localhost:3000/api/data` (Proxied)
    - Prometheus: `http://localhost:9090`
    - Grafana: `http://localhost:3001` (User: `admin`, Pass: `admin`)

### CI/CD Setup
Refer to `JENKINS_SETUP.md` for setting up Jenkins on your VPS.

## Environment Variables
Refer to `SECRETS.md` for required environment variables.
