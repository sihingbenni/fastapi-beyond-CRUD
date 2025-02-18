# FastAPI Beyond CRUD 

This is a forked repo of the original [FastAPI Beyond CRUD](https://github.com/jod35/fastapi-beyond-CRUD) repository. The original repository is a source code for the [FastAPI Beyond CRUD](https://youtube.com/playlist?list=PLEt8Tae2spYnHy378vMlPH--87cfeh33P&si=rl-08ktaRjcm2aIQ) course.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Prerequisites](#prerequisites)
3. [Project Setup](#project-setup)
4. [Running the Application](#running-the-application)
5. [Running Tests](#running-tests)
6. [Contributing](#contributing)

## Getting Started
Follow the instructions below to set up and run your FastAPI project.

### Prerequisites
Ensure you have the following installed:

- Python >= 3.10
- PostgreSQL
- Redis

### Project Setup
1. Clone the project repository:
    ```bash
    git clone https://github.com/jod35/fastapi-beyond-CRUD.git
    ```
   
2. Navigate to the project directory:
    ```bash
    cd fastapi-beyond-CRUD/
    ```

3. Create and activate a virtual environment:
    ```bash
    python3 -m venv env
    source env/bin/activate
    ```

4. Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```

5. Set up environment variables by copying the example configuration:
    ```bash
    cp .env.example .env
    ```

6. Run database migrations to initialize the database schema:
    ```bash
    alembic upgrade head
    ```

7. Open a new terminal and ensure your virtual environment is active. Start the Celery worker (Linux/Unix shell):
    ```bash
    sh runworker.sh
    ```

## Running the Application
Start the application:

```bash
fastapi dev src/
```
Alternatively, you can run the application using Docker:
```bash
docker compose up -d
```
## Running Tests
Run the tests using this command

(!! You need to install pytest before that)
```bash
pytest
```

## Contributing
I welcome contributions to improve the documentation! You can contribute [here](https://github.com/sihingbenni/fastapi-beyond-crud-docs).

Make sure to follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification when making commits and naming your PRs.
! PRs that don't follow this pattern will be closed.
e.g.:
- Features must start with: `feat:`
- Bugfixes must start with: `fix:` 
- Documentation must start with: `docs:`
- ... and so on.

## Nightly Build
This repo contains a workflow that builds the docker image and pushes it to the docker hub every night. The image is tagged with the current date and time. The workflow can be found [here](.github/workflows/nightly-build.yml).
For it to work you need to add the following secrets to your GitHub repository:
- `DOCKER_HUB_USERNAME`
- `DOCKER_HUB_PASSWORD`
- `SMTP_USERNAME`
- `SMTP_PASSWORD`
- `MAIL_TO`
- `MAIL_BCC`

Also make sure to add all your environment variables and secrets to a environment named: `.env` in GitHub
So a .env file can be created from those: 
```bash
          echo "DATABASE_URL=${{ secrets.DATABASE_URL }}" > .env
          echo "JWT_SECRET=${{ secrets.JWT_SECRET }}" >> .env
          echo "JWT_ALGORITHM=${{ secrets.JWT_ALGORITHM }}" >> .env
          echo "REDIS_HOST=${{ vars.REDIS_HOST }}" >> .env
          echo "REDIS_PORT=${{ vars.REDIS_PORT }}" >> .env
          echo "REDIS_URL=${{ secrets.REDIS_URL }}" >> .env
          echo "MAIL_USERNAME=${{ secrets.MAIL_USERNAME }}" >> .env
          echo "MAIL_PASSWORD=${{ secrets.MAIL_PASSWORD }}" >> .env
          echo "MAIL_SERVER=${{ vars.MAIL_SERVER }}" >> .env
          echo "MAIL_PORT=${{ vars.MAIL_PORT }}" >> .env
          echo "MAIL_FROM=${{ vars.MAIL_FROM }}" >> .env
          echo "MAIL_FROM_NAME=${{ vars.MAIL_FROM_NAME }}" >> .env
          echo "DOMAIN=${{ vars.DOMAIN }}" >> .env
```