# Ansible Playbooks Collection

A curated collection of production-ready Ansible playbooks for Linux server administration, infrastructure provisioning, container orchestration, web application deployment, and security hardening.

---

## 📁 Repository Overview

| Category | Playbook | Description |
| :--- | :--- | :--- |
| **Connectivity & Baseline** | [`ping.yml`](ping.yml) | Verify connectivity across all managed nodes using Ansible ping |
| | [`user.yml`](user.yml) | Provision a single DevOps administrator user |
| | [`cmul.yml`](cmul.yml) | Create multiple users, user groups, and configure secure SSH home directories |
| | [`create-directory.yml`](create-directory.yml) | Ensure project directories exist with standard permissions |
| | [`copy.yml`](copy.yml) | Deploy static HTML files to web server document root |
| **Web Servers** | [`install-nginx.yml`](install-nginx.yml) | Install and start Nginx HTTP server on web tier |
| | [`install-apache.yml`](install-apache.yml) | Install and enable Apache HTTP server (`apache2`) |
| | [`dauJinja.yml`](dauJinja.yml) | Deploy Jinja2 templated configuration file with service notification handler |
| | [`setup-ssl-letsencrypt.yml`](setup-ssl-letsencrypt.yml) | **(New)** Automate SSL/TLS certificate issuing, 2048-bit DH params, and auto-renewal |
| **Databases & Caching** | [`setup-mysql.yml`](setup-mysql.yml) | Install MySQL Server, set root password, provision application DB/user, remove anonymous users |
| | [`setup-postgresql.yml`](setup-postgresql.yml) | **(New)** Install PostgreSQL 16, create database, user with encrypted password, privileges, and `pg_hba.conf` |
| | [`setup-redis-server.yml`](setup-redis-server.yml) | **(New)** Install Redis cache server, configure max memory, LRU eviction policy, authentication, and health check |
| **Containers & Apps** | [`deploy_docker_app.yml`](deploy_docker_app.yml) | Install Docker engine and run standalone container |
| | [`deploy-nodejs-app.yml`](deploy-nodejs-app.yml) | End-to-end Node.js deployment with NodeSource, Git clone, NPM install, and Systemd service |
| | [`setup-docker-compose.yml`](setup-docker-compose.yml) | **(New)** Deploy multi-container microservice stack (Nginx + Redis + volumes + bridge network) with Docker Compose |
| **Security & Maintenance** | [`firewall.yml`](firewall.yml) | Configure and enable UFW firewall rules for web traffic |
| | [`secure-ssh.yml`](secure-ssh.yml) | Harden OpenSSH daemon (disable root login, disable passwords, change port, fail2ban) |
| | [`manage-cron-jobs.yml`](manage-cron-jobs.yml) | Automate scheduled cron jobs (log rotation, backups, disk alert checks, temp cleanup) |
| | [`system-monitoring.yml`](system-monitoring.yml) | Comprehensive health check (CPU, RAM, disk, services, uptime) and file report generation |
| | [`system-patching-reboot.yml`](system-patching-reboot.yml) | **(New)** Fleet OS patching, package cleanup, and controlled conditional reboot if required |

---

## 🚀 Newly Added Playbooks

### 1. `setup-postgresql.yml`
- **Purpose**: Fully automated PostgreSQL database server setup.
- **Features**: Installs PostgreSQL server and `python3-psycopg2`, starts and enables service, creates application database, configures database user credentials, grants all privileges, and tunes `listen_addresses` and `pg_hba.conf` authentication with handler restart.

### 2. `setup-redis-server.yml`
- **Purpose**: In-memory caching and session storage configuration.
- **Features**: Installs Redis server and CLI tools, binds to secure interfaces, configures maximum memory threshold (`256mb`) and eviction policy (`allkeys-lru`), sets up access authentication (`requirepass`), and runs automated ping verification.

### 3. `setup-ssl-letsencrypt.yml`
- **Purpose**: Zero-touch TLS/SSL certificate provisioning and renewal.
- **Features**: Installs Certbot and web server plugins, generates 2048-bit Diffie-Hellman parameters for enhanced security, requests certificates via standalone/certonly mode, schedules automated bi-weekly renewal cron job with web server reload hook, and tests dry-run renewal.

### 4. `system-patching-reboot.yml`
- **Purpose**: Automated enterprise Linux OS maintenance.
- **Features**: Updates package indices, applies distribution security patches (`upgrade: dist`), cleans unneeded dependencies (`autoremove`/`autoclean`), evaluates `/var/run/reboot-required`, conditionally initiates a managed server reboot with connection verification, and outputs post-patch uptime.

### 5. `setup-docker-compose.yml`
- **Purpose**: Production-grade multi-container orchestration.
- **Features**: Sets up Docker and the official `docker-compose-plugin`, establishes a dedicated stack directory (`/opt/microservices-stack`), writes `.env` environment variables, creates a multi-service `docker-compose.yml` (Nginx reverse proxy + Redis cache with persistent volumes and bridge network), pulls images, and launches the stack in detached mode.

---

## 🛠️ Usage Instructions

### Run a Playbook
```bash
ansible-playbook -i hosts.ini <playbook-name>.yml
```

### Dry-run / Check Mode
```bash
ansible-playbook -i hosts.ini <playbook-name>.yml --check
```

### Syntax Validation
```bash
ansible-playbook --syntax-check <playbook-name>.yml
```
