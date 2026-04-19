# 🏠 Self-Hosted Kubernetes Home-Lab

> A production-grade, GitOps-powered Kubernetes cluster demonstrating enterprise DevOps practices, infrastructure as code, and full-stack observability — deployed to `portfolio.eshundev.online`

![Status](https://img.shields.io/badge/status-active-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![Kubernetes](https://img.shields.io/badge/Kubernetes-1.19+-326CE5?logo=kubernetes&logoColor=white)

---

## 🎯 Project Overview

This repository contains the complete infrastructure-as-code for a self-hosted Kubernetes cluster running real-world applications with enterprise-grade patterns:

- ✅ **Production Portfolio Site** — WordPress at `portfolio.eshundev.online`
- ✅ **Microservices** — Custom SMS service (MG-QuickSMS)
- ✅ **Full Observability** — Prometheus, Grafana, Loki monitoring stack
- ✅ **Secure Access** — Cloudflare Tunnels with no exposed ports
- ✅ **GitOps Automation** — ArgoCD for continuous deployment
- ✅ **Infrastructure as Code** — 100% declarative Kubernetes manifests

**Why it matters:** This demonstrates **not just knowing DevOps, but living it** — designing, deploying, and operating real systems in production.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Internet Users                              │
└────────────────────────────┬────────────────────────────────────┘
                             │
                    portfolio.eshundev.online
                             │
        ┌────────────────────v────────────────────┐
        │      Cloudflare Tunnel (Secure)         │
        │     (No exposed ports / No firewall)    │
        └────────────────────┬────────────────────┘
                             │
        ┌────────────────────v─────────────────────────┐
        │   Gateway API (Kubernetes routing layer)    │
        │   HTTPRoute / TCPRoute definitions          │
        └────────────────────┬──────────────────────────┘
                             │
        ┌────────────────────v─────────────────────────┐
        │          Kubernetes Cluster (Local)         │
        │  ┌──────────────┐  ┌──────────────────────┐ │
        │  │  WordPress   │  │   MG-QuickSMS       │ │
        │  │  (Port 80)   │  │   (External repo)   │ │
        │  └──────┬───────┘  └──────────────────────┘ │
        │  ┌──────v──────────────┐                     │
        │  │  MySQL + PostgreSQL │                     │
        │  │  (Persistent Storage)                     │
        │  └────────────────────┘                      │
        │                                              │
        │  ┌────────────────────────────────────────┐ │
        │  │   Observability Stack                  │ │
        │  │  ┌──────────────────────────────────┐ │ │
        │  │  │ Prometheus (Metrics Collection) │ │ │
        │  │  │ Grafana (Dashboards)             │ │ │
        │  │  │ Loki (Log Aggregation)           │ │ │
        │  │  │ AlertManager → Slack Alerts      │ │ │
        │  │  └──────────────────────────────────┘ │ │
        │  └────────────────────────────────────────┘ │
        │                                              │
        │  ┌────────────────────────────────────────┐ │
        │  │     ArgoCD (GitOps Sync)               │ │
        │  │  Continuous deployment from git repo  │ │
        │  │  Auto-healing • Auto-pruning         │ │
        │  └────────────────────────────────────────┘ │
        └──────────────────────────────────────────────┘
```

---

## 🚀 Key Technologies

| Category                    | Technology                   | Purpose                                           |
| --------------------------- | ---------------------------- | ------------------------------------------------- |
| **Container Orchestration** | Kubernetes (1.19+)           | Deployment and management                         |
| **GitOps / CI-CD**          | ArgoCD                       | Continuous deployment from git                    |
| **Package Management**      | Helm                         | Monitoring stack provisioning                     |
| **Security**                | Sealed Secrets               | Encrypted secrets in git                          |
| **External Access**         | Cloudflare Tunnels           | Secure routing without port forwarding            |
| **Routing**                 | Gateway API                  | Modern Kubernetes ingress (HTTPRoute, TCPRoute)   |
| **Metrics**                 | Prometheus                   | Time-series metrics collection (30-day retention) |
| **Visualization**           | Grafana                      | Dashboards & alerts (NodePort 32000)              |
| **Logging**                 | Loki                         | Log aggregation & querying                        |
| **Alerting**                | AlertManager                 | Alert routing to Slack webhooks                   |
| **Applications**            | WordPress, MySQL, PostgreSQL | Content & data storage                            |

---

## 📊 Skills Demonstrated

### **DevOps / SRE**

- ✅ Kubernetes cluster design and configuration
- ✅ GitOps workflow with ArgoCD (automated sync, self-healing, pruning)
- ✅ Infrastructure as Code (100% declarative manifests)
- ✅ Storage management (PersistentVolumes, StorageClasses)
- ✅ Network design (Namespaces, Services, Gateway API)

### **Observability & Monitoring**

- ✅ Prometheus metrics collection and scraping
- ✅ Grafana dashboard design
- ✅ Centralized logging with Loki
- ✅ Alert routing to external systems (Slack integration)
- ✅ Multi-layer observability stack architecture

### **Security & Best Practices**

- ✅ Secret management (Sealed Secrets encryption)
- ✅ Secure external access (Cloudflare Tunnels)
- ✅ Zero-port-exposure design
- ✅ RBAC and namespace isolation
- ✅ Persistent secret storage in git

### **Application Architecture**

- ✅ Multi-tier application design
- ✅ Database failover (MySQL + PostgreSQL)
- ✅ Microservices integration (external SMS service)
- ✅ High availability patterns
- ✅ Persistent data management

---

## 📁 Project Structure

```
argocd-apps/
├── applications/                 # ArgoCD Application resources (entry points)
│   ├── cloudflared-app.yaml     # Cloudflare Tunnel management
│   ├── wordpress-app.yaml       # Portfolio site
│   ├── mysql-app.yaml           # MySQL database
│   ├── postgres-app.yaml        # PostgreSQL database
│   ├── observability-stack.yaml # Prometheus/Grafana/Loki/AlertManager
│   └── mg-quick-sms.yaml        # Custom SMS microservice
│
└── apps/                         # Actual service manifests
    ├── cloudflare-tunnel/        # Tunnel daemon + sealed secrets
    ├── gateway-api/              # Ingress routing rules
    ├── monitoring/               # Observability stack Helm values
    ├── mysql/                    # Database + PVC + sealed secrets
    ├── postgresql/               # Alternate database option
    └── wordpress/                # Portfolio application + PVC
```

---

## 🔑 Key Features

### **GitOps Automation**

Every change is tracked in git. ArgoCD continuously reconciles the cluster to match the desired state:

```yaml
syncPolicy:
  automated:
    prune: true # Remove resources deleted from git
    selfHeal: true # Periodic sync to desired state
  syncOptions:
    - CreateNamespace=true
```

### **Security Through Encryption**

Secrets are encrypted with Sealed Secrets — only committed `*-sealed-secret.yaml` files are in git:

```yaml
# Only this gets committed to git (encrypted)
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  name: mysql-credentials
spec:
  encryptedData:
    password: AgBvJ8K3... # encrypted
```

### **Modern Routing with Gateway API**

Using Kubernetes-native Gateway API instead of older Ingress:

```yaml
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: wordpress-route
spec:
  parentRefs:
    - name: main-gateway
  hostnames:
    - "portfolio.eshundev.online"
  rules:
    - backendRefs:
        - name: wordpress
          port: 80
```

### **Full Observability Stack**

- **Prometheus**: Scrapes metrics from all services (30-day retention)
- **Grafana**: Custom dashboards (accessible via NodePort)
- **Loki**: Centralized log aggregation
- **AlertManager**: Routes alerts to Slack for on-call notifications

---

## ⚙️ Storage Architecture

Designed for reliability with persistent storage:

| Application  | Size | Type           | Mount Path               |
| ------------ | ---- | -------------- | ------------------------ |
| WordPress    | 5Gi  | manual-storage | /var/www/html            |
| MySQL        | 5Gi  | manual-storage | /var/lib/mysql           |
| PostgreSQL   | 10Gi | manual-storage | /var/lib/postgresql/data |
| Prometheus   | 20Gi | manual-storage | /prometheus              |
| Grafana      | 5Gi  | manual-storage | /var/lib/grafana         |
| Loki         | 10Gi | manual-storage | /loki                    |
| AlertManager | 2Gi  | manual-storage | /alertmanager            |

---

## 🌍 Live Demo

**Portfolio Site:** [`portfolio.eshundev.online`](https://portfolio.eshundev.online)

**Grafana Dashboard:** Accessible internally via Kubernetes port-forward (NodePort 32000)

---

## 🛠️ Setup & Deployment

### Prerequisites

- Kubernetes cluster (1.19+)
- ArgoCD installed in `argocd` namespace
- `kubectl` configured to access cluster
- Sealed Secrets controller deployed

### Deploy the Stack

1. **Install Sealed Secrets (if not already installed):**

   ```bash
   kubectl apply -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.24.0/sealed-secrets-0.24.0.yaml
   ```

2. **Install ArgoCD (if not already installed):**

   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

3. **Deploy all applications:**

   ```bash
   kubectl apply -f argocd-apps/applications/
   ```

4. **Watch sync progress:**

   ```bash
   argocd app list
   argocd app wait cloudflared-app
   argocd app wait wordpress-app
   argocd app wait observability-stack
   ```

5. **Access Grafana dashboard:**
   ```bash
   kubectl port-forward -n monitoring svc/prometheus-grafana 3000:80
   # Then visit http://localhost:3000
   ```

---

## 📈 Metrics & Monitoring

The observability stack provides:

- **30-day metric retention** in Prometheus
- **Real-time dashboards** in Grafana
- **Centralized logs** queryable in Loki
- **Automated alerts** to Slack for anomalies

Example metrics collected:

- Container CPU/Memory usage
- Network I/O
- Pod restart counts
- Application response times
- Database query performance

---

## 🎓 Learning Outcomes

Building this home-lab provided hands-on experience with:

✅ Kubernetes architecture and networking  
✅ ArgoCD GitOps workflows  
✅ Production observability practices  
✅ Secret management and security  
✅ Infrastructure automation  
✅ DevOps tooling and best practices  
✅ Troubleshooting distributed systems  
✅ Cost optimization through local deployment

---

## 📋 What's Happening Under the Hood

1. **Git as Single Source of Truth**

   - All infrastructure defined in this repo
   - ArgoCD watches for changes and auto-deploys

2. **Secure External Access**

   - Cloudflare Tunnel provides public HTTPS without exposing cluster
   - Gateway API handles internal routing
   - No firewall rules or port forwarding needed

3. **Self-Healing Cluster**

   - ArgoCD periodically syncs desired state
   - Failed pods automatically recreated
   - Deleted resources restored from git

4. **Full Visibility**
   - Every application metrics visible in Prometheus
   - Logs centralized in Loki
   - Grafana dashboards for on-call team

---

## 🔐 Security Practices

- ✅ **Sealed Secrets**: Encrypted credentials in git
- ✅ **Network Policies**: Namespace isolation
- ✅ **RBAC**: Limited service account permissions
- ✅ **Secret Rotation**: Automated via sealed-secrets
- ✅ **Audit Logging**: All changes tracked in git

---

## 🚧 Future Enhancements

- [ ] Implement network policies for zero-trust
- [ ] Add automated backups for databases
- [ ] Integrate ArgoCD Notifications for deployment status
- [ ] Implement cost tracking via Kubecost
- [ ] Add security scanning (Trivy) in pipeline
- [ ] Implement Blue-Green deployments
- [ ] Add disaster recovery procedures

---

## 📚 Related Projects & Technologies

- **ArgoCD**: https://argoproj.github.io/cd/
- **Kubernetes Gateway API**: https://gateway-api.sigs.k8s.io/
- **Sealed Secrets**: https://github.com/bitnami-labs/sealed-secrets
- **Cloudflare Tunnels**: https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/
- **Prometheus**: https://prometheus.io/
- **Loki**: https://grafana.com/oss/loki/

---

## 💼 Why This Matters for Teams

This project demonstrates the ability to:

- **Design** enterprise-grade infrastructure
- **Implement** production patterns (GitOps, observability, security)
- **Operate** complex distributed systems
- **Automate** manual DevOps processes
- **Think strategically** about system architecture
- **Learn independently** without being told what to do

These are the skills SREs, Platform Engineers, and DevOps leaders are hired to have.

---

## 📧 Contact & Links

- **Portfolio**: [portfolio.eshundev.online](https://portfolio.eshundev.online)
- **GitHub**: [github.com/eshundev](https://github.com/eshundev)
- **LinkedIn**: [Connect with me]()

---

## 📄 License

This project is licensed under the MIT License — see LICENSE file for details.

---

**⭐ If you found this interesting, please consider starring the repo! Questions? Feel free to open an issue.**
