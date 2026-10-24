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
| | [`setup-haproxy-loadbalancer.yml`](setup-haproxy-loadbalancer.yml) | **(New)** High-availability HAProxy load balancer with health checks, stats telemetry UI, and kernel network tuning |
| **Databases & Messaging** | [`setup-mysql.yml`](setup-mysql.yml) | Install MySQL Server, set root password, provision application DB/user, remove anonymous users |
| | [`setup-postgresql.yml`](setup-postgresql.yml) | Install PostgreSQL 16, create database, user with encrypted password, privileges, and `pg_hba.conf` |
| | [`setup-mongodb.yml`](setup-mongodb.yml) | Deploy MongoDB 7.0 Community Server with SCRAM authentication, bind security, and logrotate |
| | [`setup-redis-server.yml`](setup-redis-server.yml) | Install Redis cache server, configure max memory, LRU eviction policy, authentication, and health check |
| | [`setup-rabbitmq.yml`](setup-rabbitmq.yml) | Install RabbitMQ message broker, enable web management UI, configure vhosts, users, and memory limits |
| | [`setup-elasticsearch.yml`](setup-elasticsearch.yml) | **(New)** Deploy Elasticsearch 8.x node with JVM heap options, systemd LimitMEMLOCK, and cluster health checks |
| **Containers & CI/CD** | [`deploy_docker_app.yml`](deploy_docker_app.yml) | Install Docker engine and run standalone container |
| | [`deploy-nodejs-app.yml`](deploy-nodejs-app.yml) | End-to-end Node.js deployment with NodeSource, Git clone, NPM install, and Systemd service |
| | [`setup-docker-compose.yml`](setup-docker-compose.yml) | Deploy multi-container microservice stack (Nginx + Redis + volumes + bridge network) with Docker Compose |
| | [`setup-github-actions-runner.yml`](setup-github-actions-runner.yml) | Automated GitHub Actions self-hosted runner provisioning with systemd daemon and toolchain |
| | [`setup-k8s-containerd.yml`](setup-k8s-containerd.yml) | **(New)** Kubernetes cluster node preparation, swap disablement, CRI containerd runtime, and locked kubelet/kubeadm |
| **Monitoring & Backup** | [`setup-prometheus-grafana.yml`](setup-prometheus-grafana.yml) | **(New)** Full observability stack with Prometheus server, Grafana OSS, auto-provisioned datasources, and health APIs |
| | [`setup-node-exporter.yml`](setup-node-exporter.yml) | Install Prometheus Node Exporter agent, systemd service, custom textfile collectors, and firewall rule |
| | [`backup-database-s3.yml`](backup-database-s3.yml) | Automated multi-database (MySQL/PostgreSQL) backup engine with gzip, SHA256 checksums, S3 sync, and cron |
| | [`system-monitoring.yml`](system-monitoring.yml) | Comprehensive health check (CPU, RAM, disk, services, uptime) and file report generation |
| **Security & Maintenance** | [`firewall.yml`](firewall.yml) | Configure and enable UFW firewall rules for web traffic |
| | [`secure-ssh.yml`](secure-ssh.yml) | Harden OpenSSH daemon (disable root login, disable passwords, change port, fail2ban) |
| | [`setup-vault-server.yml`](setup-vault-server.yml) | **(New)** HashiCorp Vault secrets management server with mlock memory security, file storage, and systemd daemon |
| | [`manage-cron-jobs.yml`](manage-cron-jobs.yml) | Automate scheduled cron jobs (log rotation, backups, disk alert checks, temp cleanup) |
| | [`system-patching-reboot.yml`](system-patching-reboot.yml) | Fleet OS patching, package cleanup, and controlled conditional reboot if required |

---

## 🚀 Newly Added Enterprise Playbooks

### 1. `setup-prometheus-grafana.yml`
- **Category**: Monitoring & Observability
- **Features**: Installs standalone Prometheus server (binary download with release checksums, dedicated system user/group, optimized systemd unit) and Grafana OSS (via official GPG-signed APT repository). Implements automatic datasource provisioning so Grafana instantly connects to Prometheus upon initialization, configures scrape jobs (Prometheus + Node Exporter), and validates health endpoints (`/-/healthy` and `/api/health`).

### 2. `setup-haproxy-loadbalancer.yml`
- **Category**: Web Servers & Ingress
- **Features**: Deploys an enterprise HAProxy load balancer with high-throughput Linux kernel sysctl optimizations (`net.ipv4.ip_nonlocal_bind`, `net.core.somaxconn=65535`). Features round-robin routing across backend application nodes with active HTTP health checks (`rise 2 fall 3`), isolated syslog event logging (`/var/log/haproxy.log`), and a password-protected web stats dashboard on port 8404.

### 3. `setup-k8s-containerd.yml`
- **Category**: Containers & Kubernetes
- **Features**: Automates Kubernetes cluster node preparation. Permanently disables Linux swap (`swapoff -a` and `/etc/fstab` neutralization), loads essential kernel modules (`overlay`, `br_netfilter`), applies bridge sysctl networking flags, configures containerd with `SystemdCgroup = true`, adds the official Kubernetes package repository (`v1.30`), installs `kubelet`, `kubeadm`, `kubectl`, and locks versions with `dpkg_selections`.

### 4. `setup-elasticsearch.yml`
- **Category**: Databases & Search
- **Features**: Provisions Elasticsearch 8.x from official Elastic APT repositories. Tunes memory management with `vm.max_map_count=262144`, configures security limits (`memlock unlimited`, `nofile 65536`), applies a systemd override for `LimitMEMLOCK=infinity`, allocates dedicated JVM heap sizes (`-Xms2g -Xmx2g`), and verifies cluster health via the REST API.

### 5. `setup-vault-server.yml`
- **Category**: Security & Secrets Management
- **Features**: Installs HashiCorp Vault server from the official HashiCorp release repository. Establishes an unprivileged `vault` system user, assigns `cap_ipc_lock=+ep` to prevent secrets from swapping to disk, provisions file/raft storage with secure `0750` directory permissions, sets up systemd service unit with hardened security restrictions, and registers global `VAULT_ADDR` profile environments.

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
