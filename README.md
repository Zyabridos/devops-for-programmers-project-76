# Ansible Container Orchestrator

This project uses **Ansible** to automate the provisioning and deployment of containerized applications (currently for this example I am using Redmine image) across multiple servers. It installs required system tools like `pip` and the `docker` Python module on a group of web servers.

### Hexlet tests and linter status:
[![Actions Status](https://github.com/Zyabridos/devops-for-programmers-project-76/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/Zyabridos/devops-for-programmers-project-76/actions)

## Requirements
- Python 3.x
- Ansible 2.10+
- Two configured servers (DigitalOcean or similar)
- SSH access with root privileges
- Your public SSH key must be added to each server
- Docker installed on both servers
- Load balancer configured on DigitalOcean

## Setup Instructions

1. **Ping the webservers group**:
Pings all servers in the `webservers` group to verify SSH connectivity and Ansible setup.

```bash
make ping
```
Expected output:

```json
web-1 | SUCCESS => {
  "changed": false,
  "ping": "pong"
}
web-2 | SUCCESS => {
  "changed": false,
  "ping": "pong"
}
```

2. **Install required roles, ping servers, prepare them:**

```bash
make setup
```

This command will: 
- Install external Ansible roles
- Ping all servers in webservers group
- Install pip, Docker and prepare servers for deployment

3. **Run the setup using Makefile**:
Ensure `inventory.ini` contains your correct server IPs and SSH users.

Example:
```ini
[webservers]
web-1 ansible_host=159.223.13.142 ansible_user=root
web-2 ansible_host=159.223.223.227 ansible_user=root
```

```bash
make deploy
```
This will:
- Generate a .env file for Redmine
- Remove existing Redmine container (if any)
- Deploy a new Redmine container with Bitnami image
- Configure Redmine environment automatically

Redmine will be available at:

- http://159.223.13.142:3000
- http://159.223.223.227:3000


## Configure Load Balancer (DigitalOcean)

- Add forwarding rule: HTTPS 443 → Droplet Port 3000
- Attach domain: `ansible-container-orchestrator.online`
- Use Let's Encrypt for automatic SSL certificate
- Point domain A-record to the load balancer IP

## 🔗 Link to Deployed Application
👉 https://ansible-container-orchestrator.online

## 📌 Notes
- Main playbook name should be **playbook.yml**
- Inventory file name is **inventory.ini**
- Hosts group must be called `webservers`
- Ensure your servers allow SSH access from your machine
- Redmine runs in a Docker container using the Bitnami image
- PostgreSQL is hosted separately and must be accessible from both webservers
- Secrets like DB password are stored in an encrypted file (`vault.yml`)
