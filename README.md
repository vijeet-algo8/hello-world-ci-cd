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

1. Open GCP Console > `Compute Engine` > `VM instances`.
2. Find your VM row and copy the `External IP`, which will look like `34.93.120.45`.
3. Click the `SSH` button in the same VM row to open the browser SSH terminal.
4. Wait until the SSH terminal shows a prompt like `username@vm-name:~$`.
5. In that SSH terminal, create the SSH folder with `mkdir -p ~/.ssh && chmod 700 ~/.ssh`.
6. Open the authorized keys file with `nano ~/.ssh/authorized_keys`.
7. Paste the public key from your local command `cat ~/.ssh/hello-world-ci-cd.pub` into `~/.ssh/authorized_keys` on a new line.
8. Save nano with `Ctrl + O`, press `Enter`, then exit with `Ctrl + X`.
9. Fix key permissions with `chmod 600 ~/.ssh/authorized_keys`.
10. Install Docker with `sudo apt update && sudo apt install -y docker.io docker-compose-plugin`.
11. Allow your SSH terminal user to run Docker with `sudo usermod -aG docker $USER`.
12. Close the browser SSH terminal and click `SSH` again so the Docker group change is active.
13. Clone the repository with `git clone git@github.com:vijeet-algo8/hello-world-ci-cd.git ~/hello-world-ci-cd`.
14. Go inside the project with `cd ~/hello-world-ci-cd`.
15. Start the app with `docker compose up -d --build`.
16. Check the container with `docker ps`, and the output should show `hello-world-ci-cd` with port `0.0.0.0:5004->80/tcp`.
17. Allow GCP firewall traffic on TCP port `5004` from `VPC network` > `Firewall` if the browser cannot reach the app.
18. Open `http://34.93.120.45:5004`, replacing `34.93.120.45` with your VM external IP.
19. Wow, the Hello World page is visible on `http://VM-IP:5004`.
