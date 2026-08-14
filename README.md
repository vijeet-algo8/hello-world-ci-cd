# Hello World CI/CD

## On Local System

1. Clean the repository folder and keep only required files: `index.html`, `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.gitignore`, and `README.md`.
2. Check the repository status with `git status`, and the output should show `On branch main` and `nothing to commit, working tree clean`.
3. Create a deployment SSH key with `ssh-keygen -t ed25519 -C "github-actions-vm-deploy" -f ~/.ssh/hello-world-ci-cd`.
4. Copy the public key with `cat ~/.ssh/hello-world-ci-cd.pub`, and it will look like `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... github-actions-vm-deploy`.
5. Copy the private key with `cat ~/.ssh/hello-world-ci-cd`, and it will start with `-----BEGIN OPENSSH PRIVATE KEY-----`.
6. Push code changes with `git add .`, `git commit -m "Update deployment docs"`, and `git push origin main`.

## On GitHub

1. Open the repository at `https://github.com/vijeet-algo8/hello-world-ci-cd`.
2. Go to `Settings` > `Secrets and variables` > `Actions` > `New repository secret`.
3. Add `VM_HOST` with the VM external IP value like `34.93.120.45`.
4. Add `VM_USER` with the VM username value like `ubuntu` or your GCP VM login user.
5. Add `VM_SSH_KEY` with the full private key from `cat ~/.ssh/hello-world-ci-cd`, including `BEGIN OPENSSH PRIVATE KEY` and `END OPENSSH PRIVATE KEY`.
6. Add `VM_DEPLOY_PATH` with the server folder path like `/home/ubuntu/hello-world-ci-cd`.
7. Go to the `Actions` tab and run the deployment workflow, or push to `main` if the workflow runs automatically.
8. Confirm the workflow output shows successful SSH connection, repository pull, Docker build, and container start.

## On Virtual Machine

1. Open GCP Console > `Compute Engine` > `VM instances` and copy the VM external IP, which will look like `34.93.120.45`.
2. SSH into the VM with `ssh ubuntu@34.93.120.45`, replacing `ubuntu` and the IP with your actual VM user and external IP.
3. Open the authorized keys file with `nano ~/.ssh/authorized_keys`.
4. Paste the public key from `cat ~/.ssh/hello-world-ci-cd.pub` into `~/.ssh/authorized_keys` on a new line.
5. Install Docker with `sudo apt update && sudo apt install -y docker.io docker-compose-plugin`.
6. Allow your user to run Docker with `sudo usermod -aG docker $USER`, then log out and SSH back in.
7. Clone the repository with `git clone git@github.com:vijeet-algo8/hello-world-ci-cd.git /home/ubuntu/hello-world-ci-cd`.
8. Go inside the project with `cd /home/ubuntu/hello-world-ci-cd`.
9. Start the app with `docker compose up -d --build`.
10. Check the container with `docker ps`, and the output should show `hello-world-ci-cd` with port `0.0.0.0:5004->80/tcp`.
11. Allow GCP firewall traffic on TCP port `5004` from `VPC network` > `Firewall` if the browser cannot reach the app.
12. Open `http://34.93.120.45:5004`, replacing `34.93.120.45` with your VM external IP.
13. Wow, the Hello World page is visible on `http://VM-IP:5004`.
