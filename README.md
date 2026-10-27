# Ansible Playbooks Collection

A curated collection of production-ready Ansible playbooks for Linux server administration, infrastructure provisioning, container orchestration, web application deployment, database management, monitoring, and security hardening.

---

## 📁 Repository Overview

| Category | Playbook | Description |
| :--- | :--- | :--- |
| **Connectivity & Baseline** | [`ping.yml`](ping.yml) | Verify connectivity across all managed nodes with multi-target latency benchmarking |
| | [`user.yml`](user.yml) | Provision a hardened DevOps administrator user with Ed25519 keys and PAM limits |
| | [`cmul.yml`](cmul.yml) | Multi-team provisioning, SGID collaboration workspaces, and sudo access controls |
| | [`create-directory.yml`](create-directory.yml) | Directory provisioning with POSIX ACL enforcement and capacity assertions |
| | [`copy.yml`](copy.yml) | Production static asset engine with SHA256 integrity checks, custom 404/500/503 templates, and manifests |
| **Web Servers & Ingress** | [`install-nginx.yml`](install-nginx.yml) | High-concurrency Nginx with rate-limiting zones, security headers, and JSON logging |
| | [`install-apache.yml`](install-apache.yml) | Apache HTTP server with MPM Event tuning, HTTP/2 protocol, and hardened configs |
| | [`dauJinja.yml`](dauJinja.yml) | Dynamic Jinja2 microservice orchestrator with multi-environment profiles and systemd sandboxing |
| | [`setup-ssl-letsencrypt.yml`](setup-ssl-letsencrypt.yml) | Automate SSL/TLS certificate issuing, 2048-bit DH params, and auto-renewal |
| | [`setup-haproxy-loadbalancer.yml`](setup-haproxy-loadbalancer.yml) | Enterprise HAProxy load balancer with stick-table rate limiting, security headers, stats UI, socket telemetry, and kernel tuning |
| **Databases & Messaging** | [`setup-mysql.yml`](setup-mysql.yml) | Production MySQL with InnoDB tuning, slow query profiling, SSL/TLS, and least-privilege accounts |
| | [`setup-postgresql.yml`](setup-postgresql.yml) | PostgreSQL 16 server with WAL archiving, autovacuum tuning, replication role, and SCRAM-SHA-256 |
| | [`setup-mongodb.yml`](setup-mongodb.yml) | MongoDB 7.0 Community Server with kernel THP disablement, SCRAM auth, and WiredTiger tuning |
| | [`setup-redis-server.yml`](setup-redis-server.yml) | In-memory cache with active defragmentation, Redis 7+ ACL user controls, and systemd ulimits |
| | [`setup-rabbitmq.yml`](setup-rabbitmq.yml) | High-availability RabbitMQ broker with Erlang cookie security, HA queue policies, and health diagnostics |
| | [`setup-elasticsearch.yml`](setup-elasticsearch.yml) | Elasticsearch 8.x node with dynamic JVM heap calculation, snapshot repository config, and cluster health checks |
| **Containers & CI/CD** | [`deploy_docker_app.yml`](deploy_docker_app.yml) | Hardened Docker container deployment with capability drops, resource quotas, and HTTP health probes |
| | [`deploy-nodejs-app.yml`](deploy-nodejs-app.yml) | End-to-end Node.js deployment with NodeSource toolchain, systemd security, and healthcheck verification |
| | [`setup-docker-compose.yml`](setup-docker-compose.yml) | Multi-tier microservice stack with network isolation, health dependencies, and compose validation |
| | [`setup-github-actions-runner.yml`](setup-github-actions-runner.yml) | Hardened GitHub Actions self-hosted CI/CD runner with release checksums and systemd sandboxing |
| | [`setup-k8s-containerd.yml`](setup-k8s-containerd.yml) | Kubernetes cluster node preparation, swap disablement, CRI containerd runtime, and crictl integration |
| **Monitoring & Backup** | [`setup-prometheus-grafana.yml`](setup-prometheus-grafana.yml) | Full observability stack with Prometheus, Grafana OSS, promtool alert rule validation, and dashboard provisioning |
| | [`setup-node-exporter.yml`](setup-node-exporter.yml) | Hardened Prometheus Node Exporter with systemd security isolation, TLS/basic-auth, and custom textfile metrics |
| | [`backup-database-s3.yml`](backup-database-s3.yml) | Multi-database (MySQL, PostgreSQL, MongoDB, Redis) backup engine with S3 SSE encryption and flock locking |
| | [`system-monitoring.yml`](system-monitoring.yml) | Comprehensive health check with top process analytics, port auditing, threshold evaluation, and alert dispatchers |
| **Security & Maintenance** | [`firewall.yml`](firewall.yml) | Stateful UFW firewall with kernel TCP SYN cookie mitigation, application profiles, and ICMP rate limiting |
| | [`secure-ssh.yml`](secure-ssh.yml) | Hardened OpenSSH daemon with ChaCha20/Curve25519 cryptographic suite, warning banners, and Fail2Ban |
| | [`setup-vault-server.yml`](setup-vault-server.yml) | HashiCorp Vault secrets management server with Raft HA storage, memory locking, and audit logging |
| | [`manage-cron-jobs.yml`](manage-cron-jobs.yml) | Scheduled cron automation with flock concurrency locks, log health checker, and crontab audits |
| | [`system-patching-reboot.yml`](system-patching-reboot.yml) | Fleet OS patching, rolling reboot orchestration, needrestart audit, and health verification |

---

## 🚀 Newly Added Enterprise Playbooks

### 1. `setup-prometheus-grafana.yml`
- **Category**: Monitoring & Observability
- **Features**: Installs standalone Prometheus server (binary download with release checksums, dedicated system user/group, optimized systemd sandboxing unit) and Grafana OSS (via official GPG-signed APT repository). Validates config and alert rules using `promtool check config`, implements automatic datasource & dashboard provisioning so Grafana instantly connects to Prometheus upon initialization, configures scrape jobs (Prometheus + Node Exporter), and validates health endpoints (`/-/healthy` and `/api/health`).

### 2. `setup-haproxy-loadbalancer.yml`
- **Category**: Web Servers & Ingress
- **Features**: Deploys an enterprise HAProxy load balancer with high-throughput Linux kernel sysctl optimizations (`net.ipv4.ip_nonlocal_bind`, `net.core.somaxconn=65535`). Features round-robin routing across backend application nodes with active HTTP health checks, anti-abuse stick-table rate limiting (`http_req_rate(10s)`), custom error templates (429/503), runtime statistics inspection via `/run/haproxy/admin.sock`, and a password-protected web stats dashboard on port 8404.

### 3. `setup-k8s-containerd.yml`
- **Category**: Containers & Kubernetes
- **Features**: Automates Kubernetes cluster node preparation. Permanently disables Linux swap (`swapoff -a` and `/etc/fstab` neutralization), loads essential kernel modules (`overlay`, `br_netfilter`), applies bridge & inotify sysctl networking flags, configures containerd with `SystemdCgroup = true` and pinned pause images (`registry.k8s.io/pause:3.9`), provisions `/etc/crictl.yaml`, adds the official Kubernetes package repository (`v1.30`), installs `kubelet`, `kubeadm`, `kubectl`, and locks versions with `dpkg_selections`.

### 4. `setup-elasticsearch.yml`
- **Category**: Databases & Search
- **Features**: Provisions Elasticsearch 8.x from official Elastic APT repositories. Tunes memory management with `vm.max_map_count=262144`, dynamically computes optimal JVM heap allocation (50% system RAM), configures security limits (`memlock unlimited`, `nofile 65536`), applies a systemd override for `LimitMEMLOCK=infinity`, registers snapshot backup directories (`path.repo`), and verifies cluster health via the REST and Cat APIs.

### 5. `setup-vault-server.yml`
- **Category**: Security & Secrets Management
- **Features**: Installs HashiCorp Vault server from the official HashiCorp release repository. Establishes an unprivileged `vault` system user, assigns `cap_ipc_lock=+ep` to prevent secrets from swapping to disk, provisions Raft HA or file storage with secure `0700` directory permissions, sets up systemd service unit with hardened security restrictions, enables Prometheus telemetry metrics, configures `/var/log/vault` audit logging with logrotate, and registers global `VAULT_ADDR` profile environments and shell completion.

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
