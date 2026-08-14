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

1. Open GCP Console > `Compute Engine` > `VM instances`.
2. Find your VM row and copy the `External IP`, which will look like `34.93.120.45`.
3. Click the `SSH` button in the same VM row to open the browser SSH terminal.
4. Wait until the SSH terminal shows a prompt like `username@vm-name:~$`.
5. Check the current VM username with `whoami`, and use this exact value as the GitHub secret `VM_USER`.
6. Create the SSH folder with `mkdir -p ~/.ssh && chmod 700 ~/.ssh`.
7. Open authorized keys with `nano ~/.ssh/authorized_keys`.
8. Paste the local public key from `cat ~/.ssh/hello-world-ci-cd.pub` into `~/.ssh/authorized_keys` on a new line.
9. Save nano with `Ctrl + O`, press `Enter`, then exit with `Ctrl + X`.
10. Fix SSH permissions with `chmod 600 ~/.ssh/authorized_keys`.
11. Install Docker and Docker Compose with `sudo apt update && sudo apt install -y docker.io docker-compose-plugin git`.
12. Allow the current VM user to run Docker with `sudo usermod -aG docker $USER`.
13. Close the browser SSH terminal and click `SSH` again so Docker group access is active.
14. Confirm Docker works with `docker --version`, and the output should look like `Docker version ...`.
15. Confirm Docker Compose works with `docker compose version`, and the output should look like `Docker Compose version ...`.
16. If this repository is private, create the VM-to-GitHub key with `ssh-keygen -t ed25519 -C "vm-github-read-key" -f ~/.ssh/id_ed25519`.
17. If this repository is private, show the VM public key with `cat ~/.ssh/id_ed25519.pub` and add it in GitHub > `Settings` > `Deploy keys`.
18. Test GitHub SSH from the VM with `ssh -T git@github.com`, and a successful output includes `successfully authenticated`.
19. Clone the repository with `git clone git@github.com:vijeet-algo8/hello-world-ci-cd.git ~/hello-world-ci-cd`.
20. If the folder already exists, update it with `cd ~/hello-world-ci-cd && git pull origin main`.
21. Go inside the project with `cd ~/hello-world-ci-cd`.
22. Start the app with `docker compose up -d --build`.
23. Check the container with `docker ps`, and the expected port output is `0.0.0.0:5004->80/tcp`.
24. Test inside the VM with `curl http://localhost:5004`, and the output should include `Hello world`.
25. Firewall rule for TCP port `5004` is already created on the VM/GCP network, so no new firewall rule is needed.
26. If the browser still cannot reach port `5004`, verify GCP Console > `VPC network` > `Firewall` has an ingress allow rule for `tcp:5004`.
27. If the firewall rule uses a target tag, confirm the same tag is added from `Compute Engine` > `VM instances` > your VM > `Edit` > `Network tags`.
28. Open `http://34.93.120.45:5004`, replacing `34.93.120.45` with your VM external IP.
29. Wow, the Hello World page is visible on `http://VM-IP:5004`.
