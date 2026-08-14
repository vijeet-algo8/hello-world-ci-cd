# Hello World CI/CD

This repository serves a simple `Hello world` page using Docker and Docker Compose on port `5004`.

## Deployment Flow

| Step | Local System | GitHub | Virtual Machine |
|---|---|---|---|
| Step 1 | Keep only required files: `index.html`, `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.gitignore`, and `README.md`. |  |  |
| Step 2 | Run `git status` and confirm the output says `On branch main` and `nothing to commit, working tree clean`. |  |  |
| Step 3 | Run `git remote -v` and confirm it shows `git@github.com:vijeet-algo8/hello-world-ci-cd.git`. |  |  |
| Step 4 | Create the GitHub-to-VM deployment key: `ssh-keygen -t ed25519 -C "github-actions-vm-deploy" -f ~/.ssh/hello-world-ci-cd` and press `Enter` twice for no passphrase. |  |  |
| Step 5 | Copy the public key with `cat ~/.ssh/hello-world-ci-cd.pub`; it looks like `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... github-actions-vm-deploy`. |  |  |
| Step 6 | Copy the private key with `cat ~/.ssh/hello-world-ci-cd`; it starts with `-----BEGIN OPENSSH PRIVATE KEY-----` and ends with `-----END OPENSSH PRIVATE KEY-----`. |  |  |
| Step 7 |  | Open `https://github.com/vijeet-algo8/hello-world-ci-cd`. |  |
| Step 8 |  | Confirm these files exist: `index.html`, `Dockerfile`, `docker-compose.yml`, `.dockerignore`, `.gitignore`, and `README.md`. |  |
| Step 9 |  | Go to `Settings` > `Secrets and variables` > `Actions` > `New repository secret`. |  |
| Step 10 |  | Add `VM_HOST` with the VM external IP, for example `34.93.120.45`. |  |
| Step 11 |  | Add `VM_USER` with the VM SSH terminal username, for example `ubuntu` or the value from `whoami` on the VM. |  |
| Step 12 |  | Add `VM_PORT` with value `22` unless the VM SSH port was changed. |  |
| Step 13 |  | Add `VM_SSH_KEY` with the full private key from `cat ~/.ssh/hello-world-ci-cd`, including the begin and end lines. |  |
| Step 14 |  | Add `VM_DEPLOY_PATH` with the VM project path, for example `/home/ubuntu/hello-world-ci-cd`. |  |
| Step 15 |  |  | Paste this in the VM SSH terminal: `whoami` |
| Step 16 |  |  | Paste this in the VM SSH terminal: `mkdir -p ~/.ssh && chmod 700 ~/.ssh && touch ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys` |
| Step 17 |  |  | Paste this in the VM SSH terminal after replacing `<PASTE_LOCAL_PUBLIC_KEY_HERE>` with the local public key: `echo '<PASTE_LOCAL_PUBLIC_KEY_HERE>' >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys` |
| Step 18 |  |  | Paste this in the VM SSH terminal: `sudo apt update && sudo apt install -y docker.io docker-compose-plugin git && sudo systemctl enable --now docker` |
| Step 19 |  |  | Paste this in the VM SSH terminal: `sudo docker --version && sudo docker compose version` |
| Step 20 |  |  | Paste this in the VM SSH terminal: `if [ ! -f ~/.ssh/id_ed25519 ]; then ssh-keygen -t ed25519 -C "vm-github-read-key" -f ~/.ssh/id_ed25519 -N ""; fi` |
| Step 21 |  |  | Paste this in the VM SSH terminal: `cat ~/.ssh/id_ed25519.pub` |
| Step 22 |  | Go to `Settings` > `Deploy keys` > `Add deploy key`, paste the VM public key from Step 21, and keep `Allow write access` unchecked. |  |
| Step 23 |  |  | Paste this in the VM SSH terminal: `ssh -o StrictHostKeyChecking=accept-new -T git@github.com || true` |
| Step 24 |  |  | Paste this in the VM SSH terminal: `if [ -d ~/hello-world-ci-cd ]; then cd ~/hello-world-ci-cd && git pull origin main; else git clone git@github.com:vijeet-algo8/hello-world-ci-cd.git ~/hello-world-ci-cd && cd ~/hello-world-ci-cd; fi` |
| Step 25 |  |  | Paste this in the VM SSH terminal: `sudo docker compose up -d --build` |
| Step 26 |  |  | Paste this in the VM SSH terminal: `sudo docker ps` and confirm the port shows `0.0.0.0:5004->80/tcp`. |
| Step 27 |  |  | Paste this in the VM SSH terminal: `curl http://localhost:5004` and confirm the output includes `Hello world`. |
| Step 28 |  |  | Open `http://VM-IP:5004` in the browser; the firewall rule for TCP port `5004` is already created on the VM/GCP network. |
| Step 29 |  |  | Wow, the Hello World page is visible on `http://VM-IP:5004`. |

For automated CI/CD later, add `.github/workflows/deploy-vm.yml` and use the GitHub secrets from Steps 10 to 14.
