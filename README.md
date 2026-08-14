# Hello World CI/CD

## On Local System

1. Clean the repository folder and keep only required project files.
2. Run `git status` and confirm only expected files are present.
3. Run `git add .` to stage the project files.
4. Run `git commit -m "Initial commit"` to create the commit.
5. Run `git push origin main` to push the code to GitHub.

## On GitHub

1. Open `https://github.com/vijeet-algo8/hello-world-ci-cd`.
2. Confirm `index.html`, `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.gitignore`, and `README.md` are present.
3. Add the required CI/CD workflow or deployment action for the VM.
4. Add VM access secrets such as host, username, SSH key, and deployment path.
5. Run the workflow and confirm the deployment job completes successfully.

## On Virtual Machine

1. SSH into the GCP VM.
2. Install Docker and Docker Compose if they are not already installed.
3. Pull or clone the latest repository code on the VM.
4. Go inside the repository folder.
5. Run `docker compose up -d --build`.
6. Run `docker ps` and confirm `hello-world-ci-cd` is running.
7. Open `http://VM-IP:5004`.
8. Wow, the Hello World page is visible on `http://VM-IP:5004`.
