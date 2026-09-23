# Ansible Playbooks Collection

A curated collection of production-ready Ansible playbooks for Linux server administration, infrastructure provisioning, container orchestration, web application deployment, database management, monitoring, and security hardening.

---

## 📁 Repository Overview

| Category | Playbook | Description |
| :--- | :--- | :--- |
| **Connectivity & Baseline** | [`ping.yml`](ping.yml) | Verify connectivity across all managed nodes using Ansible ping |
| | [`user.yml`](user.yml) | Provision a single DevOps administrator user |
| | [`cmul.yml`](cmul.yml) | Create multiple users, user groups, and configure secure SSH home directories |
| | [`create-directory.yml`](create-directory.yml) | Ensure project directories exist with standard permissions |
| | [`copy.yml`](copy.yml) | Deploy static HTML files to web server document root |
| **Web Servers & Ingress** | [`install-nginx.yml`](install-nginx.yml) | Install and start Nginx HTTP server on web tier |
| | [`install-apache.yml`](install-apache.yml) | Install and enable Apache HTTP server (`apache2`) |
| | [`dauJinja.yml`](dauJinja.yml) | Deploy Jinja2 templated configuration file with service notification handler |
| | [`setup-ssl-letsencrypt.yml`](setup-ssl-letsencrypt.yml) | Automate SSL/TLS certificate issuing, 2048-bit DH params, and auto-renewal |
| **Databases & Messaging** | [`setup-mysql.yml`](setup-mysql.yml) | Install MySQL Server, set root password, provision application DB/user, remove anonymous users |
| | [`setup-postgresql.yml`](setup-postgresql.yml) | Install PostgreSQL 16, create database, user with encrypted password, privileges, and `pg_hba.conf` |
| | [`setup-mongodb.yml`](setup-mongodb.yml) | **(New)** Deploy MongoDB 7.0 Community Server with SCRAM authentication, bind security, and logrotate |
| | [`setup-redis-server.yml`](setup-redis-server.yml) | Install Redis cache server, configure max memory, LRU eviction policy, authentication, and health check |
| | [`setup-rabbitmq.yml`](setup-rabbitmq.yml) | **(New)** Install RabbitMQ message broker, enable web management UI, configure vhosts, users, and memory limits |
| **Containers & CI/CD** | [`deploy_docker_app.yml`](deploy_docker_app.yml) | Install Docker engine and run standalone container |
| | [`deploy-nodejs-app.yml`](deploy-nodejs-app.yml) | End-to-end Node.js deployment with NodeSource, Git clone, NPM install, and Systemd service |
| | [`setup-docker-compose.yml`](setup-docker-compose.yml) | Deploy multi-container microservice stack (Nginx + Redis + volumes + bridge network) with Docker Compose |
| | [`setup-github-actions-runner.yml`](setup-github-actions-runner.yml) | **(New)** Automated GitHub Actions self-hosted runner provisioning with systemd daemon and toolchain |
| **Monitoring & Backup** | [`setup-node-exporter.yml`](setup-node-exporter.yml) | **(New)** Install Prometheus Node Exporter agent, systemd service, custom textfile collectors, and firewall rule |
| | [`backup-database-s3.yml`](backup-database-s3.yml) | **(New)** Automated multi-database (MySQL/PostgreSQL) backup engine with gzip, SHA256 checksums, S3 sync, and cron |
| | [`system-monitoring.yml`](system-monitoring.yml) | Comprehensive health check (CPU, RAM, disk, services, uptime) and file report generation |
| **Security & Maintenance** | [`firewall.yml`](firewall.yml) | Configure and enable UFW firewall rules for web traffic |
| | [`secure-ssh.yml`](secure-ssh.yml) | Harden OpenSSH daemon (disable root login, disable passwords, change port, fail2ban) |
| | [`manage-cron-jobs.yml`](manage-cron-jobs.yml) | Automate scheduled cron jobs (log rotation, backups, disk alert checks, temp cleanup) |
| | [`system-patching-reboot.yml`](system-patching-reboot.yml) | Fleet OS patching, package cleanup, and controlled conditional reboot if required |

---

## 🚀 Newly Added Playbooks

### 1. `setup-node-exporter.yml`
- **Purpose**: Prometheus Node Exporter system metrics monitoring daemon.
- **Features**: Installs latest Prometheus Node Exporter binary, establishes a dedicated `node_exporter` system user and group, deploys an optimized systemd service unit with collector flags, enables custom textfile collector support (`/var/lib/node_exporter/textfile_collector`), configures UFW firewall rule for TCP port 9100, and verifies HTTP scrape endpoint health.

### 2. `backup-database-s3.yml`
- **Purpose**: Enterprise-grade multi-database backup, integrity validation, and cloud archival.
- **Features**: Performs live consistent dumps of MySQL (`mysqldump` with `--single-transaction`) and PostgreSQL (`pg_dump -Fc`), applies level-9 gzip compression, calculates SHA256 cryptographic verification checksums, synchronizes archives to AWS S3 or compatible object storage with `STANDARD_IA` tier, executes automated retention pruning of expired backups, and schedules daily cron execution at 02:00 UTC.

### 3. `setup-rabbitmq.yml`
- **Purpose**: High-performance RabbitMQ message broker deployment.
- **Features**: Installs required Erlang/OTP dependencies and official RabbitMQ Server, enables `rabbitmq_management` and `rabbitmq_prometheus` plugins, configures memory high watermark limits and disk limits in `/etc/rabbitmq/rabbitmq.conf`, provisions administrator credentials and dedicated application worker users with scoped virtual host (`/production`) permissions, removes the insecure default `guest` account, and exposes AMQP (5672) and management console (15672) ports.

### 4. `setup-mongodb.yml`
- **Purpose**: Production MongoDB 7.0 Community Edition database server.
- **Features**: Configures official MongoDB GPG key and APT repository, installs `mongodb-org` suite, deploys secured `mongod.conf` (enforcing SCRAM authorization, 1GB WiredTiger cache sizing, and localhost binding), provisions the administrative root user (`siteAdmin`) and application database read/write user (`app_user`), and sets up logrotate policies to cycle logs with SIGUSR1 signals.

### 5. `setup-github-actions-runner.yml`
- **Purpose**: Scalable self-hosted GitHub Actions CI/CD runner.
- **Features**: Provisions full developer toolchain dependencies (`build-essential`, `libssl-dev`, `jq`, `git`, `python3-pip`), creates isolated `actions-runner` user with Docker group access, downloads and extracts official runner binaries, executes runner OS dependency scripts, registers the runner against the target repository with custom tags and unattended mode, deploys a systemd daemon service, and enables auto-start on system boot.

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
