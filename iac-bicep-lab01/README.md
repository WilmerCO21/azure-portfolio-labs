# IaC Project — Lab 01 with Bicep (Azure)

## Goal
Deploy an Ubuntu VM + Networking + NSG (SSH restricted) + Public IP using **Bicep** and Azure CLI, then destroy everything to control costs.

## Prerequisites
- Azure CLI installed
- Bicep (Azure CLI installs it automatically on first use)
- An SSH key pair

## Steps (Windows PowerShell)

### 1) Login and create RG
```powershell
az login
az group create -n rg-iac-lab01 -l eastus

