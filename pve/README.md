# Proxmox Virtual Environment Setup

Proxmox Virtual Environment - Home Lab Setup

## Proxmox Virtual Environment Installation

Download Proxmox from [here](https://www.proxmox.com/en/proxmox-virtual-environment/get-started) and follow the instructions to write to an ISO or USB boot drive

## Setup

Add a role for Terraform to use
```
pveum role add terraform-role -privs "VM.Allocate VM.Clone VM.Config.CDROM VM.Config.CPU VM.Config.Cloudinit VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Monitor VM.Audit VM.PowerMgmt Datastore.AllocateSpace Datastore.Audit Sys.Audit Datastore.Allocate SDN.Use Sys.Modify"
```
Add a local PVE user
```
pveum user add f-terraform@pve -password <password-here> -comment "Terraform Functional User"
```
Add the role we create to the new user
```
pveum aclmod / -user f-terraform@pve -role terraform-role
```
Update `.bashrc` with the three environment variables
```
export PROXMOX_VE_USERNAME="f-terraform@pve"
export PROXMOX_VE_PASSWORD="<the-password-above>"
export PROXMOX_VE_ENDPOINT="https://<hostname-or-ip>:8006/"
```

Add a user on the host server for SSH access
```
useradd f-terraform
passwd f-terraform
chmod 777 /var/lib/vz/snippets
```
There should be a better way!!!!!

## References

### The BPG Proxmox Terraform Provider

- [BPG Proxmox Provider](https://registry.terraform.io/providers/bpg/proxmox/latest/docs)
- [BPG Examples](https://github.com/bpg/terraform-provider-proxmox/tree/main/example)

### Reference repositories

- [My home operations repo using K8](https://github.com/szinn/k8s-homelab)
- [Small Home Lab for K8 using PVE](https://github.com/ehlesp/smallab-k8s-pve-guide)

### Secondary References

- [Cloud Init Template Builder Script](https://gist.github.com/chris2k20)
- [Cloud-Init file reference](https://github.com/chris2k20/proxmox-cloud-init/tree/main)
- [ProxMox Cloud Init Examples](https://dustinrue.com/2020/05/going-deeper-with-proxmox-cloud-init/)