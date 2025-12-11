# Django Chess Application

A web-based chess application built with Django that allows users to play chess online, track their games, and improve their skills.

## Features

* User authentication and profiles
* Real-time chess gameplay
* Game history and statistics
* Move validation and legal move highlighting
* Save and resume games
* Basic chess AI for single-player mode
* ELO rating system

---

## Prerequisites

Before you begin, ensure you have installed:

* Python 3.8+ (for local development without Docker)
* pip (Python package manager)
* Git
* Docker and Docker Compose (for containerized setup)
* Optional but Recommended: **uv** (fast Python package & environment manager)

> ⚠️ If you plan to use Docker, you **don't need a local Python environment** — everything runs inside containers.

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/django-chess.git
cd django-chess
```

---

### 2. Set up environment variables

```bash
# Create .env file in the project root
cp .env.example .env
```

Edit `.env` with your configuration:

```env
SECRET_KEY=your_secret_key
DEBUG=True
DATABASE_URL=postgres://postgres:password@db:5432/chessplay
STOCKFISH_PATH=/usr/local/bin/stockfish
```

> **Note:** If using Docker, the database hostname is `db` (the service name in docker-compose), not `localhost`.

---

## ⚡ Installing uv (Optional but Recommended)

**uv** is a fast Python package and environment manager from Astral (creators of Ruff).

### Option 1: Script Installer (Recommended)

**macOS/Linux:**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows (PowerShell):**
```powershell
winget install --id=astral-sh.uv -e
```

### Option 2: Install via pip

```bash
pip install uv
```

---

## 🖥️ Local Development with uv

uv automatically manages virtual environments for you, but you can also create them explicitly.

### Option 1: Automatic Environment (Recommended)

uv creates and manages the environment automatically:

```bash
# Install dependencies (creates .venv automatically)
uv sync -r requirements.txt

# Run migrations
uv run python manage.py migrate

# Create superuser
uv run python manage.py createsuperuser

# Start development server
uv run python manage.py runserver
```

### Option 2: Explicit Environment Creation

Create a virtual environment explicitly with uv:

```bash
# Create virtual environment
uv venv

# Activate the environment
source .venv/bin/activate  # Linux/macOS
.venv\Scripts\activate     # Windows

# Install dependencies
uv pip install -r requirements.txt

# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Start development server
python manage.py runserver
```

Visit: `http://localhost:8000`

### Managing dependencies with uv

**Add a new package:**
```bash
uv pip install <package-name>
```

**Export dependencies:**
```bash
uv pip freeze > requirements.txt
```

**Re-sync after changes:**
```bash
uv sync -r requirements.txt
```

---

## 🖥️ Local Development without uv (Traditional Method)

If you prefer the traditional approach:

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt

# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Start server
python manage.py runserver
```

> ⚠️ Recommended only for small testing. Docker ensures all dependencies and Postgres connectivity are consistent.

---

## 🐳 Docker Setup (Recommended for Production)

### Build Docker Image

```bash
# Option 1: Build with Compose
docker compose build

# Option 2: Build manually
docker build -t django-chess-app .
```

* `docker build` → builds the image from Dockerfile
* `-t django-chess-app` → tags the image
* `docker compose build` → builds all services defined in docker-compose.yml

---

### Run the application

```bash
# Run all services (Django + Postgres)
docker compose up

# Run in detached mode
docker compose up -d
```

Visit: `http://localhost:8000`

---

### Running Django Commands inside Docker

After Dockerizing, **all management commands must run inside the container**:

```bash
# Run migrations
docker compose exec web python manage.py migrate

# Create superuser
docker compose exec web python manage.py createsuperuser

# Open Django shell
docker compose exec web python manage.py shell
```

> ⚠️ Commands like `python manage.py runserver` **won't work on host** once Docker is in use.

---

### 🔄 Using uv inside Docker

The container includes uv, so you can use it for faster package management:

```bash
# Run migrations with uv
docker compose exec web uv run python manage.py migrate

# Create superuser with uv
docker compose exec web uv run python manage.py createsuperuser

# Open Django shell with uv
docker compose exec web uv run python manage.py shell

# Install a package inside container
docker compose exec web uv pip install <package-name>
```

---

### Viewing Logs

```bash
docker compose logs -f web    # Django logs
docker compose logs -f db     # Postgres logs
```

---

### Stop & Remove Containers

```bash
docker compose down          # Stop containers
docker compose down -v       # Stop containers and remove volumes (DB reset)
```

---

### Run Container Without Compose (Optional)

```bash
docker run --rm -it -p 8000:8000 --env-file .env django-chess-app
```

* `--rm` → auto-remove container when stopped
* `-it` → interactive + TTY
* `-p 8000:8000` → map container port to host
* `--env-file .env` → load environment variables

---

## Project Structure

```
saaschess/
├── app_name/                  # Main application directory
│   ├── models.py              # Database models
│   ├── views.py               # View functions
│   ├── urls.py                # URL configurations
│   └── templates/             # HTML templates
├── static/                    # Static files (CSS, JS, images)
├── manage.py                  # Django management script
├── requirements/              # Python dependencies
├── Dockerfile                 # Docker build instructions
├── docker-compose.yml         # Docker services
├── .env                       # Environment variables
└── README.md                  # This file
```

---

## Making Migrations

After modifying models:

**With Docker:**
```bash
docker compose exec web python manage.py makemigrations
docker compose exec web python manage.py migrate
```

**With uv (local):**
```bash
uv run python manage.py makemigrations
uv run python manage.py migrate
```

**Traditional (local):**
```bash
python manage.py makemigrations
python manage.py migrate
```

---

## Commands Cheat Sheet

### Docker Commands

| Command | Purpose |
| ------- | ------- |
| `docker compose build` | Build all services via Compose |
| `docker compose up` | Start containers (Django + Postgres) |
| `docker compose up -d` | Run containers in background |
| `docker compose down` | Stop containers |
| `docker compose down -v` | Stop containers + remove volumes |
| `docker compose exec web python manage.py <command>` | Run Django management commands |
| `docker compose exec web uv run python manage.py <command>` | Run Django commands using uv inside container |
| `docker compose logs -f web` | View Django logs |
| `docker compose logs -f db` | View Postgres logs |
| `docker run --rm -it -p 8000:8000 --env-file .env django-chess-app` | Run container manually |
| `docker images` | List local images |
| `docker rmi <image_name>` | Remove local image |
| `docker ps` | List running containers |
| `docker stop <container_id>` | Stop a container |

### uv Commands

| Command | Purpose |
| ------- | ------- |
| `uv venv` | Create virtual environment |
| `uv sync -r requirements.txt` | Install/sync dependencies (auto-creates .venv) |
| `uv run python manage.py <command>` | Run Django commands |
| `uv pip install <package>` | Install a package |
| `uv pip freeze > requirements.txt` | Export dependencies |
| `uv pip list` | List installed packages |

---

## Acknowledgments

* [Django Documentation](https://docs.djangoproject.com/)
* [python-chess](https://python-chess.readthedocs.io/)
* [uv Documentation](https://docs.astral.sh/uv/)
