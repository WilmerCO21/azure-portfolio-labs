# Azure Portfolio Labs (Hands-On)

Hands-on Azure labs to demonstrate practical cloud skills for an **Azure/Cloud Intern** role.  
Each lab contains its own `README.md` with step-by-step instructions and evidence (screenshots).

## Labs Included

### Lab 01 — VM + NSG + SSH + Nginx (2 versions)

- **Lab 01A (Portal / Manual)** → [`lab-01/Portal/`](./lab-01/Portal)
  - Built using **Azure Portal** steps (manual provisioning + validation)
  - Evidence: resource list, NSG rules, SSH access, Nginx over HTTP

- **Lab 01B (IaC / Bicep + Azure CLI)** → [`lab-01/iac-bicep/`](./lab-01/iac-bicep)
  - Deploys the same architecture using **Bicep + Azure CLI**
  - Includes parameters, outputs, validation, and cleanup workflow

### Lab 02 — Azure Storage (Blob) → [`lab-02/`](./lab-02)
- Storage Account + Container
- Upload/download files and access testing

### Lab 03 — Monitoring + Alerts → [`lab-03/`](./lab-03)
- Azure Monitor alert rule (CPU threshold)
- Action Group (email)
- Alert trigger test

### Lab 04 — RBAC (IAM) → [`lab-04/`](./lab-04)
- Role assignment at Resource Group scope
- Verification of permissions

## Tech Stack
- Microsoft Azure (Portal + Azure CLI)
- Bicep (IaC)
- Ubuntu Linux
- SSH
- Nginx
- Azure Storage (Blob)
- Azure Monitor (Alerts + Action Groups)
- RBAC / IAM

## Cost & Cleanup
All labs include cleanup steps. **Always delete Resource Groups after testing** to avoid charges.

## Author
**Wilmer Curi Orosco**


