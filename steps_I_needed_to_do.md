# Steps I needed to do to get this project running on WINDOWS

## Try 1 (Running the steps out of the README.md)

1. Creating a virtual environment using PyCharm
2. Installing the required dependencies using requirements.txt
   ![img.png](images/img.png)
   One of the packages is not supported by windows...

## Try Nr. 2 (Running the steps out of the README.md) with Windows Subsystem for Linux (WSL)

1. Creating a virtual environment using `python3 -m venv ubuntu_env`
2. Activating the virtual environment using `source ubuntu_env/bin/activate`
3. Installing the required dependencies using `pip install -r requirements.txt`
4. Setting up environment variables by copying the example configuration using `cp .env.example .env`
5. Created an ethereal mail account ![img.png](images/img1.png)
6. Filled in the environment variables in the `.env` and in the `.env.example` (so the professor can run it) file
7. Running database migrations to initialize the database schema using `alembic upgrade head`
    1. An Error occurred: "sqlalchemy.exc.InvalidRequestError: The asyncio extension requires an async driver to be
       used. The loaded 'psycopg2' is not async."
    2. fixed it by running: adding: "+asyncpg" to the DATABASE_URL in the `.env` file
       DATABASE_URL=postgresql+asyncpg://postgres:testpass@db:5432/bookly
    3. Next Error: "socket.gaierror: [Errno -3] Temporary failure in name resolution"
    4. Fixed it by starting docker: `docker-compose up` (in Windows terminal)
        1. Next Error: "failed to solve: archive/tar: unknown file mode ?rwxr-xr-x" whilst building the web app
        2. Solved by building the web app using the wsl
        3. Had to enable docker in the wsl2 ubuntu image I have installed.
        4. Next Error: "permission denied" whilst building the web app
        5. Solved by adding the user to the docker group: `sudo usermod -aG docker $USER`
        6. Hurray! Except The original Error is still there!
            1. Fixed by changing the DATABASE_URL to
               `DATABASE_URL=postgresql+asyncpg://postgres:testpass@localhost:5432/bookly`
            2. This way it connects to localhost instead of the internal docker hostname
            3. Make sure to change it when running it in docker
8. Success the migrations are done!
9. After changing the REDIS_URL to `REDIS_URL=redis://localhost:6379`
10. We are able to run the Celery worker using `sh runworker.sh`
11. Everything is working locally

## Setting up the workflow for Linting the PR

1. Creating workflow `conventional-commits.yml` in the `.github/workflows` folder
    1. The workflow checks all the commits and gets the Email address from there.
2. Adding Secrets to the Github Repository
    1. `SMTP_USERNAME`
    2. `SMTP_PASSWORD`

### Testing the workflow

1. Created a new branch `test`
2. made changes to `steps_I_needed_to_do.md`
3. Commit message written in german
4. Created PR with bs name
    1. The workflow failed because of the PR name
5. Created PR with a valid name
    1. The workflow passed
6. Created PR with "[WIP]" infront of the name

## Changed Readme regarding Contribution

## Added Workflow for building the nightly build

1. Created a new workflow `nightly-build.yml` in the `.github/workflows` folder
2. The workflow builds the docker image and pushes it to the docker hub
3. Created Access Token in the Docker Hub
4. Added Secrets to the GitHub Repository
    1. `DOCKER_USERNAME`
    2. `DOCKER_PASSWORD`
    3. `EMAIL_TO`

### Testing the workflow
1. Triggering the workflow manually
2. The workflow failed, because the environment variables were not set
![img.png](images/img2.png)
3. But at least an Email has been sent to the specified Email address in the GitHub Secrets: ![img.png](images/img3.png)
4. Fixed by setting the environment variables in the GitHub Environment Variables ![img.png](images/img4.png)
5. Triggering the workflow manually again