# Lab 01 — Azure IaC with Bicep (Ubuntu VM + NSG + Public IP + Nginx)

This lab deploys an Ubuntu 22.04 VM and its networking (VNet/Subnet, NSG, NIC, Public IP) using **Bicep + Azure CLI**.  
Then it installs **Nginx** on the VM and verifies HTTP access from the internet.

## What gets deployed

- **VNet**: `vnet-iac-01` (`10.0.0.0/16`)
- **Subnet**: `snet-vm` (`10.0.1.0/24`)
- **NSG**: `nsg-vm-iac`
  - SSH (22) **only from your public IP** (`myIpCidr`)
  - HTTP (80) from `httpSource` (default `*` for demo; recommended to restrict)
- **Public IP** (static, Standard SKU): `pip-${vmName}`
- **NIC**: `nic-${vmName}`
- **VM**: `${vmName}` (Ubuntu 22.04 LTS Gen2)

## Parameters (from `main.bicep`)

| Name | Required | Default | Notes |
|---|---:|---|---|
| `location` | No | `resourceGroup().location` | You can still pass `location=...` to be explicit |
| `vmName` | No | `vm-iac-ubuntu-01` | VM resource name |
| `adminUsername` | No | `azureuser` | Linux admin user |
| `sshPublicKey` | **Yes** | — | Contents of your `.pub` key |
| `myIpCidr` | **Yes** | — | Your public IP in CIDR (example `38.x.x.x/32`) for SSH |
| `httpSource` | No | `*` | `*` for public demo, or set to your IP `/32` for restricted HTTP |
| `vmSize` | No | `Standard_B1s` | If the SKU is restricted/unavailable in your subscription/region, pass another size (example used: `Standard_D2s_v3`) |

> Note on SKU issues: some VM sizes can be **NotAvailableForSubscription** or **Capacity Restrictions** in certain regions.  
> In this lab we solved it by deploying to `eastus2` and overriding `vmSize`.

## Prerequisites

- Azure CLI installed and authenticated (`az login`)
- Bicep available (Azure CLI installs it automatically when needed)
- SSH key pair (example: `~/.ssh/azure_iac_lab` and `~/.ssh/azure_iac_lab.pub`)

## Steps (Windows PowerShell)

### 1) Set variables

```powershell
$loc = "eastus2"
$rg  = "rg-iac-lab01-$loc"

# Your public IP (CIDR /32)
$myIpCidr = (Invoke-RestMethod "https://api.ipify.org?format=text") + "/32"

# Your SSH public key content
$pub = Get-Content "$env:USERPROFILE\.ssh\azure_iac_lab.pub" -Raw

az group create -n $rg -l $loc
```

### 2) Validate and deploy

```powershell
# Validate
az deployment group validate `
  --resource-group $rg `
  --template-file .\main.bicep `
  --parameters location=$loc vmSize="Standard_D2s_v3" myIpCidr="$myIpCidr" sshPublicKey="$pub"

# Create
az deployment group create `
  --resource-group $rg `
  --template-file .\main.bicep `
  --parameters location=$loc vmSize="Standard_D2s_v3" myIpCidr="$myIpCidr" sshPublicKey="$pub"
```

### 3) Get the public IP and SSH

```powershell
$ip = az deployment group show -g $rg -n main --query "properties.outputs.publicIp.value" -o tsv
$ip

ssh -i "$env:USERPROFILE\.ssh\azure_iac_lab" azureuser@$ip
```

### 4) Install Nginx and verify locally (inside the VM)

```bash
sudo apt update && sudo apt -y upgrade
sudo apt -y install nginx
sudo systemctl enable --now nginx
curl -I http://localhost
```

### 5) Restrict HTTP 80 to your IP (recommended)

You can do this **either** by redeploying with `httpSource=$myIpCidr` **or** (what we did) update the rule in place:

```powershell
az network nsg rule update `
  -g $rg `
  --nsg-name nsg-vm-iac `
  -n Allow-HTTP-80 `
  --source-address-prefixes $myIpCidr
```

### 6) Verify HTTP from your machine

```powershell
Invoke-WebRequest "http://$ip" -UseBasicParsing | Select-Object -ExpandProperty StatusCode
(Invoke-WebRequest "http://$ip" -UseBasicParsing).Content.Substring(0,80)
```

## Evidence (screenshots)

All evidence screenshots are in `./screenshots`:

- `01-az-resource-list.png` — created resources
- `02-deployment-provisioningstate-succeeded.png` — deployment provisioningState
- `03-nsg-rules.png` — NSG rules (SSH + HTTP)
- `04-deployment-outputs.png` — deployment outputs (public IP / SSH command)
- `05-ssh-nginx-active-enabled.png` — nginx active + enabled (via SSH)
- `06-http-statuscode-200.png` — HTTP 200 from public IP
- `07-http-content-snippet.png` — HTML snippet (“Welcome to nginx!”)

## Cleanup (important to avoid cost)

```powershell
az group delete -n $rg --yes --no-wait
```

---

**Repository path suggestion:** `azure-portfolio-labs/iac-bicep-lab01/`  
**Author:** Wilmer Curi
