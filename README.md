# ShipIt — Containerized Flask Application with CI/CD

ShipIt is a containerized Python Flask application with an automated CI/CD pipeline using GitHub Actions.

The project demonstrates automated testing, Docker image building, and publishing container images to GitHub Container Registry (GHCR).

## Architecture

```text
Developer
    |
    v
GitHub Repository
    |
    v
GitHub Actions
    |
    +----------------------+
    |                      |
    v                      v
 CI Workflow         Publish Workflow
    |                      |
    v                      v
Run Pytest           Build Docker Image
Build Docker Image          |
                            v
                     GitHub Container
                       Registry (GHCR)
                            |
                            v
                     Pull & Run Image
```

## Technologies Used

- Python 3.12
- Flask
- Pytest
- Docker
- Docker Compose
- GitHub Actions
- GitHub Container Registry (GHCR)

## Project Structure

```text
shipit-cicd/
│
├── app.py
├── test_app.py
├── requirements.txt
├── requirements-dev.txt
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── .gitignore
│
└── .github/
    └── workflows/
        ├── ci.yml
        └── publish.yml
```

## Run Locally

### Prerequisites

- Python 3.12 or later
- Docker Desktop (for container execution)
- Git

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/shipit-cicd.git
cd shipit-cicd
```

### 2. Create a virtual environment

**Windows PowerShell:**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Linux/macOS:**

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### 4. Run tests

```bash
pytest -v
```

### 5. Start the application

```bash
python app.py
```

Open the application:

- Home: http://localhost:5000/
- Health check: http://localhost:5000/health

## Run with Docker

Build the image:

```bash
docker build -t shipit-app:local .
```

Run the container:

```bash
docker run --rm -p 5000:5000 shipit-app:local
```

Access the application at http://localhost:5000/.

## Run with Docker Compose

Start the application:

```bash
docker compose up --build
```

Stop the application:

```bash
docker compose down
```

## CI/CD Pipeline

The project uses two GitHub Actions workflows.

### Continuous Integration — `ci.yml`

Triggered by:
- Push events
- Pull requests

Pipeline activities:
- Set up Python.
- Install application and development dependencies.
- Execute automated tests using Pytest.
- Build the Docker image.

### Container Publishing — `publish.yml`

Triggered by:
- Pushes to the `main` branch.

Pipeline activities:
- Authenticate with GitHub Container Registry using `GITHUB_TOKEN`.
- Build the Docker image.
- Publish the image to GHCR.
- Tag the image with `latest` and the commit SHA.

The workflow uses the `packages: write` permission to publish container images without storing a personal access token.

## Pull and Run the Published Image

Pull the image:

```bash
docker pull ghcr.io/YOUR_GITHUB_USERNAME/shipit-cicd:latest
```

Run the container:

```bash
docker run --rm -p 5000:5000 ghcr.io/YOUR_GITHUB_USERNAME/shipit-cicd:latest
```

Replace `YOUR_GITHUB_USERNAME` with your GitHub username.

If the package is private, authenticate with GHCR before pulling the image.

## Application Endpoints

| Endpoint | Description |
|---|---|
| `/` | Application home endpoint |
| `/health` | Application health check |

## Container Security

- The Docker container runs using a non-root user.
- A Docker health check is configured.
- `.dockerignore` excludes unnecessary files from the build context.
- GitHub credentials are not hardcoded into the source code or workflows.

## Troubleshooting

**Docker daemon unavailable**

Start Docker Desktop and wait until the Docker engine is running.

**PowerShell blocks virtual environment activation**

Run:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Then activate the virtual environment again.

**GHCR publishing permission error**

Verify that:
- `packages: write` is configured in `publish.yml`.
- Repository workflow permissions allow the required access.
- The workflow uses the automatically provided `GITHUB_TOKEN`.

## Future Improvements

- Add Docker image and dependency vulnerability scanning.
- Pin GitHub Actions to specific versions or commit SHAs.
- Introduce semantic versioning for container images.
- Add automated deployment to an Azure container hosting service.
- Implement deployment approvals and rollback mechanisms.

## Project Status

**Personal DevOps project — Flask application, Docker containerization, automated CI testing, and GitHub Container Registry publishing.**
