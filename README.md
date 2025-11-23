# Zabbix GitOps – CDR Application Monitoring Template

This repository demonstrates a GitOps workflow for deploying Zabbix Templates using **GitHub Actions + Ansible**.  
Whenever the XML template is updated in Git, GitHub Actions automatically deploys it to the Zabbix Server through the Zabbix API.

---

##  Template: CDR Processing Application

**Template Name:** `Template CDR Processing Application`  
This template monitors key health indicators of a CDR (Call Detail Record) ingestion and parsing pipeline.

### **Included Monitoring Items**
| Metric | Key | Purpose |
|--------|-----|----------|
| CPU Utilization | `system.cpu.util` | Detects high CPU usage |
| CDR Service Check | `net.tcp.service[custom,3001]` | Checks if CDR collection service is running |
| CDR Backlog File Count | `vfs.dir.count[/cdr/incoming]` | Detects backlog of unparsed CDR files |

### **Triggers**
| Trigger | Condition | Priority |
|---------|-----------|----------|
| High CPU | CPU > 85% | High |
| Service Down | Port 3001 down | Disaster |
| Backlog Too High | >50 files | High |

---

##  CI/CD Workflow Overview

The Zabbix Template is deployed automatically whenever:

- `templates/template-cdr-application.xml` is modified  
- or `deploy-cdr-template.yml` is updated  

### Pipeline Flow
Git Commit → GitHub Actions → Ansible Playbook → Zabbix API → Template Updated

* Note: Store the zabbix_api & creds in github secrets:  
**Settings → Secrets → Actions**
