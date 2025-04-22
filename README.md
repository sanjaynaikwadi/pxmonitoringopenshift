# PX Monitoring Ansible Automation

This project automates the setup of **Portworx Backup monitoring on OpenShift** using Ansible. It configures:

- OpenShift User Workload Monitoring
- Prometheus and Alertmanager integration
- Custom AlertManager email template
- TLS certificate and token secrets for PX-Backup
- All interactions use `kubeconfig` and `oc login` required

---

## 📦 Features

✅ Creates required namespaces and ConfigMaps  
✅ Injects custom Alertmanager configuration and templates  
✅ Creates TLS+token secrets for Prometheus & Alertmanager  
✅ Uses Ansible best practices (roles, variables, idempotency)  

---

## 🚀 Prerequisites

- Ansible 2.12+  
- Access to the OpenShift cluster (via `kubeconfig`)  
- `oc` CLI installed and authenticated (for token creation)

Install required Ansible collection:
```bash
ansible-galaxy collection install kubernetes.core
```

---

## 📂 Directory Structure

```
px-monitoring-ansible/
├── kubeconfig/
│   └── ocp-sn-demo-72-kubeconfig             # Your OpenShift kubeconfig
├── px-prometheus-playbook.yml               # Entry-point playbook
└── roles/
    └── px_prometheus_config/
        ├── files/
        │   ├── cluster-monitoring-config.yaml
        │   ├── user-workload-monitoring-config.yaml
        │   └── pxc_template.tmpl             # HTML alert email template
        ├── meta/
        │   └── main.yml
        ├── tasks/
        │   ├── main.yml
        │   ├── create_monitoring_secrets.yml
        │   └── update_alertmanager_secret.yml
        ├── templates/
        │   └── alertmanager.yaml.j2          # (optional, not actively used)
        └── vars/
            └── main.yml                      # Default variables (e.g., pxbackup_namespace)
```

---

## ⚙️ Default Variables

Default variables are defined in:
```
roles/px_prometheus_config/vars/main.yml
```

```yaml
pxbackup_namespace: px-backup
kubeconfig_path: ~/.kube/config
```

---

## 📥 Running the Playbook

### Using default values:

```bash
Make sure you are logged in via OC command line 
```

```bash
ansible-playbook px-prometheus-playbook.yml
```

### Override namespace or kubeconfig:

```bash
ansible-playbook px-prometheus-playbook.yml \
  -e pxbackup_namespace=central \
  -e kubeconfig_path=kubeconfig/ocp-sn-demo-72-kubeconfig
```

---

## 🙌 Credits

Maintained by Sanjay N.  
Built with 💙 using Ansible + Portworx + OpenShift.

---

## 📬 Feedback

Have questions or want to contribute? Feel free to open a PR or issue.
