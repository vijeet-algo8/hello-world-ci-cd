# Hello World CI/CD

This repository serves a simple `Hello world` page using Docker and Docker Compose on port `5004`.

## On Local System

1. Keep only required project files in this folder: `index.html`, `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.gitignore`, and `README.md`.
2. Check the local Git state with `git status`, and the expected clean output is `On branch main` and `nothing to commit, working tree clean`.
3. Confirm the Git remote with `git remote -v`, and it should show `git@github.com:vijeet-algo8/hello-world-ci-cd.git`.
4. Create the GitHub-to-VM deployment SSH key with `ssh-keygen -t ed25519 -C "github-actions-vm-deploy" -f ~/.ssh/hello-world-ci-cd`.
5. When asked for a passphrase, press `Enter` twice so the CI/CD workflow can use the key non-interactively.
6. Show the public key with `cat ~/.ssh/hello-world-ci-cd.pub`, and it will look like `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... github-actions-vm-deploy`.
7. Show the private key with `cat ~/.ssh/hello-world-ci-cd`, and it will start with `-----BEGIN OPENSSH PRIVATE KEY-----` and end with `-----END OPENSSH PRIVATE KEY-----`.
8. Keep the private key secret, because it will be pasted only into GitHub repository secrets as `VM_SSH_KEY`.
9. Push any README or deployment changes with `git add .`, `git commit -m "Update deployment steps"`, and `git push origin main`.

## On GitHub

1. Open `https://github.com/vijeet-algo8/hello-world-ci-cd`.
2. Confirm the repository contains `index.html`, `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.gitignore`, and `README.md`.
3. Go to `Settings` > `Secrets and variables` > `Actions` > `New repository secret`.
4. Add `VM_HOST` with the VM external IP value, for example `34.93.120.45`.
5. Add `VM_USER` with the VM SSH terminal username, for example `ubuntu` or the username shown before `@` in `username@vm-name:~$`.
6. Add `VM_PORT` with value `22` unless the VM SSH port was changed.
7. Add `VM_SSH_KEY` with the full private key from `cat ~/.ssh/hello-world-ci-cd`, including the begin and end lines.
8. Add `VM_DEPLOY_PATH` with the VM project path, for example `/home/ubuntu/hello-world-ci-cd`.
9. If the repository is private and the VM will run `git clone git@github.com:...`, go to `Settings` > `Deploy keys` > `Add deploy key`.
10. Paste the VM public key from `cat ~/.ssh/id_ed25519.pub` into the deploy key field and keep `Allow write access` unchecked.
11. Use the VM section below for manual deployment if no GitHub Actions workflow is visible in the `Actions` tab.
12. Use the secrets above for automated CI/CD when `.github/workflows/deploy-vm.yml` is added to this repository.
13. The successful automated workflow output should show SSH connection, Docker build, Docker Compose restart, and a `curl http://localhost:5004` success check.

## On Virtual Machine

1. Copy and paste this in the VM SSH terminal to check the VM username.
   ```bash
   whoami
   ```

2. Copy and paste this in the VM SSH terminal to prepare SSH permissions.
   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   touch ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

3. Copy and paste this in the VM SSH terminal after replacing `<PASTE_LOCAL_PUBLIC_KEY_HERE>` with the output of `cat ~/.ssh/hello-world-ci-cd.pub` from the local system.
   ```bash
   echo '<PASTE_LOCAL_PUBLIC_KEY_HERE>' >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

4. Copy and paste this in the VM SSH terminal to install Docker, Docker Compose, and Git.
   ```bash
   sudo apt update
   sudo apt install -y docker.io docker-compose-plugin git
   sudo systemctl enable --now docker
   ```

5. Copy and paste this in the VM SSH terminal to check Docker and Docker Compose.
   ```bash
   sudo docker --version
   sudo docker compose version
   ```

6. Copy and paste this in the VM SSH terminal to create the VM GitHub read key.
   ```bash
   if [ ! -f ~/.ssh/id_ed25519 ]; then
     ssh-keygen -t ed25519 -C "vm-github-read-key" -f ~/.ssh/id_ed25519 -N ""
   fi
   ```

7. Copy and paste this in the VM SSH terminal to show the VM GitHub public key.
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

8. Copy and paste this in the VM SSH terminal to test GitHub SSH access.
   ```bash
   ssh -o StrictHostKeyChecking=accept-new -T git@github.com || true
   ```

9. Copy and paste this in the VM SSH terminal to clone or update the repository.
   ```bash
   if [ -d ~/hello-world-ci-cd ]; then
     cd ~/hello-world-ci-cd
     git pull origin main
   else
     git clone git@github.com:vijeet-algo8/hello-world-ci-cd.git ~/hello-world-ci-cd
     cd ~/hello-world-ci-cd
   fi
   ```

10. Copy and paste this in the VM SSH terminal to start the app on port `5004`.
    ```bash
    sudo docker compose up -d --build
    ```

11. Copy and paste this in the VM SSH terminal to check the running container.
    ```bash
    sudo docker ps
    ```

12. Copy and paste this in the VM SSH terminal to test the page inside the VM.
    ```bash
    curl http://localhost:5004
    ```
