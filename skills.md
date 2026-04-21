# SKILLS BANK

---

# INSTRUCTIONS (for Claude Code)

- Select **2–5 skill categories** based on job description relevance.
- Include **8–16 skills total** — never dump everything.
- Prefer skills tagged [strength:high] and marked Expert or Proficient.
- Match **terminology from the job description** when selecting skill labels.
- Do NOT include skills not supported by work experience, projects, or certifications.
- For each skill, use the label as-is or rephrase slightly to match JD language.
- Proficiency guide:
  - **Expert** — Used in professional/client-facing work with measurable outcomes
  - **Proficient** — Used in multiple projects or academic + professional contexts
  - **Familiar** — Used in lab, coursework, or personal projects; not production-tested

---

# SKILL CATEGORIES

---

## Cloud Platforms
[category: cloud_platforms]

- [tags: aws, cloud, strength:high, proficiency: Expert]
  Amazon Web Services — EC2, S3, RDS, VPC, IAM, ELB, Auto Scaling, CloudWatch, CloudTrail, KMS, CloudEndure

- [tags: azure, cloud, strength:high, proficiency: Expert]
  Microsoft Azure — Virtual Machines, Azure AD / Entra ID, Azure Migrate, AzCopy, ExpressRoute, Azure Database (MySQL, PostgreSQL), Defender for Cloud, Pricing Calculator

- [tags: gcp, cloud, proficiency: Proficient]
  Google Cloud Platform — Cloud Functions, Admin SDK, IAM, Google Workspace

- [tags: kubernetes, cloud_native, strength:high, proficiency: Proficient]
  Kubernetes — Helm, Pod Orchestration, Autoscaling (HPA), Health Probes, Canary Rollouts, Persistent Volumes, StatefulSets

---

## Cloud Engineering & Migration
[category: cloud_engineering]

- [tags: cloud_migration, architecture, strength:high, proficiency: Expert]
  Cloud Migration — CloudEndure, Azure Migrate, AzCopy, Lift & Shift, Application Refactoring, Hybrid Cloud Design

- [tags: cloud_discovery, asset_management, strength:high, proficiency: Expert]
  Cloud Discovery & Assessment — vCenter Agent Deployment, ServiceNow CMDB, Splunk, Hyper-V, Azure Migrate Assessment

- [tags: networking, vpc, load_balancer, strength:high, proficiency: Expert]
  Cloud Networking — VPC Design, Public/Private Load Balancers, Security Groups, NSGs, ExpressRoute, Ingress/Egress Control

- [tags: devops, terraform, ansible, iac, strength:high, proficiency: Proficient]
  Infrastructure as Code — Terraform (Azure VM Provisioning), Ansible (Configuration Management), Kompose.io

- [tags: monitoring, observability, grafana, strength:high, proficiency: Proficient]
  Monitoring & Observability — CloudWatch, CloudTrail, Grafana (Dashboards, SLA Thresholds), Prometheus, Metrics API

- [tags: cost_optimization, azure, aws, proficiency: Proficient]
  Cloud Cost Optimization — Azure Pricing Calculator, VM Right-Sizing, Auto Scaling Strategy, Reserved Instances

---

## Security Engineering & Architecture
[category: security_engineering]

- [tags: cloud_security, encryption, kms, tls, strength:high, proficiency: Expert]
  Cloud Security Architecture — KMS Encryption, TLS/SSL Integration, Secure Network Design, Shared Responsibility Model

- [tags: iam, rbac, access_control, zero_trust, strength:high, proficiency: Expert]
  Identity & Access Management — IAM Policies, RBAC, Least Privilege, PingIdentity (IdP Integration), Google Workspace IAM, Delegated Admin Models

- [tags: network_security, firewall, ids, strength:high, proficiency: Proficient]
  Network Security — Firewall Configuration (pfSense), DNS Sinkhole, VLAN Segmentation, Security Group Management, IDS (Zeek, Snort concepts)

- [tags: endpoint_security, hardening, linux, strength:high, proficiency: Proficient]
  System Hardening — Linux Permission Management, Access Control, OS Upgrades (RHEL, SUSE, CentOS), Windows Server 2019

- [tags: security_awareness, phishing, social_engineering, proficiency: Proficient]
  Security Awareness & Threat Simulation — GoPhish Campaign Design, BEC/Spear Phishing Simulation, Security Metrics Tracking

---

## Security Operations & Detection Engineering
[category: soc_detection]

- [tags: siem, detection_engineering, log_analysis, strength:high, proficiency: Proficient]
  SIEM & Detection Engineering — Log Aggregation, Alert Configuration, Multi-Source Log Integration, SIEM-like Pipeline Design

- [tags: threat_intelligence, misp, zeek, ioc, strength:high, proficiency: Proficient]
  Threat Intelligence & IDS — Zeek IDS, MISP (IoC Management), ATT&CK Intel Mapping, intel.dat Pipeline Automation

- [tags: incident_response, soc, playbooks, strength:high, proficiency: Proficient]
  Incident Response — SOC Playbook Development, Alert Triage, XDR Configuration, Escalation Workflows

- [tags: mitre_attack, d3fend, threat_mapping, strength:high, proficiency: Proficient]
  MITRE ATT&CK & D3FEND — Technique Mapping, Navigator Layer Generation, D3FEND Countermeasure Correlation, Heuristic Detection

- [tags: network_traffic_analysis, wireshark, pcap, proficiency: Proficient]
  Network Traffic Analysis — Wireshark, tshark, tcpdump, pcap Analysis, TCP Stream Reassembly, Protocol Hierarchy Analysis

---

## Application Security
[category: appsec]

- [tags: sast, dast, sca, strength:high, proficiency: Proficient]
  Application Security Testing — CodeQL (SAST), OWASP ZAP (DAST), OWASP Dependency Check (SCA), GitHub Security Integration

- [tags: web_security, api_security, owasp, strength:high, proficiency: Proficient]
  Web & API Security — SQL Injection, XSS, CSRF, IDOR, Authentication Flaws, API Misconfigurations, OWASP Top 10

- [tags: penetration_testing, exploitation, strength:high, proficiency: Proficient]
  Penetration Testing — Network Pentesting (nmap, Wireshark), Web App Testing (BUNGLE!, DVWA-style), eJPT Certified

- [tags: binary_exploitation, rop, buffer_overflow, proficiency: Familiar]
  Binary Exploitation — Stack Smashing, ROP Chains (DEP Bypass), NOP Sleds (ASLR Bypass), Heap Exploitation, GDB Debugging

- [tags: secrets_management, code_security, proficiency: Proficient]
  Secrets & Code Security — Credential Remediation, .gitignore Hardening, Exposed Secrets Detection

---

## Offensive Security & Red Team
[category: red_team]

- [tags: penetration_testing, red_team, kali, strength:high, proficiency: Proficient]
  Offensive Security Tools — Kali Linux, Mimikatz (AD Privilege Escalation), nmap, Metasploit concepts, ROPgadget

- [tags: active_directory, privilege_escalation, windows, proficiency: Proficient]
  Active Directory Attacks — Privilege Escalation (Windows Server 2019), Credential Harvesting (Mimikatz), Lateral Movement Concepts

- [tags: cryptographic_attacks, aes, rsa, sha, proficiency: Proficient]
  Cryptographic Attacks — SHA-256 Length Extension, AES CBC Padding Oracle, MD5 Collision (fastcoll), Bleichenbacher RSA Forgery

- [tags: network_attacks, mac_flooding, arp, proficiency: Familiar]
  Network Attack Simulation — MAC Flooding (Colasoft Capsa), ARP Spoofing, DNS Exfiltration, TraceRoute Flood

---

## Digital Forensics & Incident Response (DFIR)
[category: dfir]

- [tags: digital_forensics, mobile_forensics, axiom, ufed, strength:high, proficiency: Proficient]
  Digital Forensics Tools — Magnet AXIOM, Cellebrite UFED Reader, OSForensics, Google Takeout Analysis

- [tags: forensics_methodology, chain_of_custody, timeline_analysis, proficiency: Proficient]
  Forensic Methodology — Chain of Custody, Timeline Reconstruction, Multi-Source Corroboration, Evidence Integrity Verification

- [tags: osint, social_media, proficiency: Proficient]
  OSINT — Social Media Profiling (LinkedIn, Facebook, Instagram, Twitter), Identity Correlation, Open-Source Intelligence Gathering

- [tags: cloud_dfir, multi_cloud, forensics, proficiency: Familiar]
  Cloud DFIR — AWS CloudTrail, Azure Sentinel, GCP Cloud Audit Logs, Log Format Analysis (JSON/XML), Multi-Cloud Forensic Framework Design

---

## Governance, Risk & Compliance (GRC)
[category: grc]

- [tags: iso27001, audit, isms, strength:high, proficiency: Proficient]
  ISO 27001 — Annex A Controls, Audit Planning & Execution, ISMS Design, Gap Analysis (ISO/IEC 27001:2022 Lead Auditor Certified)

- [tags: nist, risk_assessment, control_mapping, strength:high, proficiency: Proficient]
  NIST Framework — Control Mapping, Risk Assessment, Gap Analysis Documentation, CSF Alignment

- [tags: gdpr, compliance, data_residency, proficiency: Proficient]
  GDPR & Regulatory Compliance — Data Residency Controls, Access Workflow Design, EU Infrastructure Compliance

- [tags: ics_security, ot, scada, proficiency: Familiar]
  OT / ICS Security — ICS/SCADA Concepts (CISA ICS 300, OPSWAT CIP), Zero Trust for OT, Incident Response in Industrial Environments

- [tags: change_management, itsm, fcr, proficiency: Expert]
  Change Management — ITSM Change Requests, Firewall Change Requests (FCR), Multi-Stakeholder Approval Coordination, Change Execution

---

## AI & Machine Learning Security
[category: ai_security]

- [tags: ai_security, llm, red_teaming, prompt_engineering, strength:high, proficiency: Proficient]
  AI Security & LLM Red Teaming — Adversarial Prompt Engineering, Implicit Elicitation, Safety Boundary Evaluation, LLM Safety Dataset Creation

- [tags: adversarial_ml, cnn, ann, strength:high, proficiency: Proficient]
  Adversarial Machine Learning — Gradient Perturbation Attacks (Targeted & Non-Targeted), CNN/ANN Resilience Benchmarking, MNIST Adversarial Testing

- [tags: deep_learning, ids, nsl_kdd, proficiency: Proficient]
  Deep Learning for Security — CNN, RNN, GRU, CNN-LSTM for IDS (99.46% accuracy on NSL-KDD), Confusion Matrix & Precision-Recall Analysis

- [tags: ml_ops, inference, autoscaling, proficiency: Familiar]
  ML Infrastructure — Helm-Managed Inference Deployment, Autoscaling Tuning, p95 Latency Optimization, GPU-Accelerated Computing (NVIDIA Certified)

---

## Programming & Automation
[category: programming]

- [tags: python, scripting, automation, strength:high, proficiency: Proficient]
  Python — Security Tooling, REST API Integration, Automation Scripts, Data Pipeline Development, Exploit Scripting

- [tags: bash, shell, linux, proficiency: Proficient]
  Bash / Shell Scripting — Linux Automation, Agent Deployment Scripts, System Administration

- [tags: api, rest, sdk, proficiency: Proficient]
  REST APIs & SDK Integration — Google Admin SDK, MISP API, MITRE ATT&CK STIX API, D3FEND API

- [tags: java, android, proficiency: Familiar]
  Java / Android — Android App Development (Android Studio, SQLite)

- [tags: php, html, javascript, proficiency: Familiar]
  Web Development — PHP, HTML5, CSS3, JavaScript, MySQL/MySQLi

- [tags: c, systems_programming, proficiency: Familiar]
  Systems Programming — C, x86 Assembly, Kernel-Level Debugging (GDB, PintOS)

---

## Tools & Platforms
[category: tools]

- [tags: docker, containers, proficiency: Proficient]
  Containerization — Docker, Docker Compose, Kompose.io, GitHub Container Registry (GHCR)

- [tags: git, github, cicd, proficiency: Proficient]
  Version Control & CI/CD — Git, GitHub Actions, CodeQL Security Scanning, Branch Management

- [tags: servicenow, cmdb, itsm, proficiency: Proficient]
  ITSM & Asset Management — ServiceNow CMDB, Splunk (Log Correlation), ITSM Workflows

- [tags: minitab, statistics, proficiency: Familiar]
  Statistical Analysis — Minitab (SPC, p-Charts, DMAIC), Descriptive Statistics, Quality Control Methods

- [tags: jira, project_management, proficiency: Familiar]
  Project Management — Jira (Issue Tracking, Agile Workflows), Documentation, Meeting Minutes

- [tags: mariadb, mysql, postgresql, proficiency: Familiar]
  Databases — MariaDB, MySQL, PostgreSQL, SQLite (Schema Design, Performance Troubleshooting)

---

# QUICK-REFERENCE TAG INDEX
> Claude Code: Use this to quickly find which category covers a JD keyword.

| JD Keyword | Category |
|---|---|
| AWS / Azure / GCP | cloud_platforms |
| Kubernetes / Helm / Docker | cloud_platforms + devops |
| Cloud migration / hybrid cloud | cloud_engineering |
| Terraform / Ansible / IaC | cloud_engineering |
| Grafana / Prometheus / monitoring | cloud_engineering |
| IAM / RBAC / Zero Trust | security_engineering |
| Firewall / network security | security_engineering |
| SIEM / detection / log analysis | soc_detection |
| MITRE ATT&CK / D3FEND | soc_detection |
| Incident response / SOC | soc_detection |
| Wireshark / pcap / network forensics | soc_detection |
| SAST / DAST / SCA / CodeQL / ZAP | appsec |
| Web security / OWASP / API security | appsec |
| Penetration testing / vuln assessment | appsec + red_team |
| Exploit development / binary / ROP | red_team |
| Active Directory / Mimikatz | red_team |
| Cryptographic attacks | red_team |
| Digital forensics / AXIOM / UFED | dfir |
| OSINT / timeline reconstruction | dfir |
| ISO 27001 / NIST / GRC / audit | grc |
| GDPR / compliance | grc |
| ICS / SCADA / OT security | grc |
| LLM / AI security / prompt injection | ai_security |
| Adversarial ML / model evaluation | ai_security |
| Deep learning / CNN / IDS | ai_security |
| Python / scripting / automation | programming |
| REST API / SDK | programming |
| Docker / GitHub Actions / CI/CD | tools |
| ServiceNow / CMDB / ITSM | tools |