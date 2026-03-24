# TP2 : Azure first steps

# I. Prérequis

## 1. Starting blocks

## 2. Une paire de clés SSH

### A. Choix de l'algorithme de chiffrement

🌞 **Déterminer quel algorithme de chiffrement utiliser pour vos clés**

```bash 
Il serait preferable d'utilisé ECDSA avec connexion SSH au lieu de RSA, comme recommandé dans le communiqué de l'ANSSI dans la recommandation 10 "Lorsque les clients et les serveurs SSH supportent ECDSA, son usage doit être préféré à RSA."
    -> D'apres mes recherches c'est parceque RSA est devenu obsolète et vulnérable a cause des attaques quantum computing.
https://cyber.gouv.fr/sites/default/files/2014/01/NT_OpenSSH.pdf

Mais je vais utiliser Ed25519, car il est plus rapide, stable et a une meilleur sécurité. 
```

### B. Génération de votre paire de clés

🌞 **Générer une paire de clés pour ce TP**

```bash 
fatma@ubuntu:~$ ls -l ~/.ssh/cloud_tp1*
-rw------- 1 fatma fatma 464 mars  24 02:22 /home/fatma/.ssh/cloud_tp1
-rw-r--r-- 1 fatma fatma  97 mars  24 02:22 /home/fatma/.ssh/cloud_tp1.pub
```

### C. Agent SSH

🌞 **Configurer un agent SSH sur votre poste**

```bash 
fatma@ubuntu:~$ eval "$(ssh-agent -s)"
Agent pid 28639
fatma@ubuntu:~$ ssh-add ~/.ssh/cloud_tp1
Enter passphrase for /home/fatma/.ssh/cloud_tp1: 
Identity added: /home/fatma/.ssh/cloud_tp1 (fatma@efrei-tp1)
fatma@ubuntu:~$ ssh-add -l
256 SHA256:9...ou18LuC5Co fatma@efrei-tp1 (ED25519)
```

# II. Spawn des VMs

## 1. Depuis la WebUI

🌞 **Connectez-vous en SSH à la VM pour preuve**

```bash 
fatma@ubuntu:~$ ssh azureuser@20.67.243.224
The authenticity of host '20.67.243.224 (20.67.243.224)' can't be established.
ED25519 key fingerprint is SHA256:6cP8rtwVUq34L5eu3+QcwwSkYWyWMF+ve1+FU2Md8xo.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '20.67.243.224' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1008-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Tue Mar 24 01:43:07 UTC 2026

  System load:  0.05              Processes:             136
  Usage of /:   5.7% of 28.02GB   Users logged in:       0
  Memory usage: 4%                IPv4 address for eth0: 10.0.0.4
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

## 2. `az` : a programmatic approach

🌞 **Créez une VM depuis le Azure CLI**

```bash 
fatma@ubuntu:~$ az vm create --resource-group parc-info-cloud --name vm-alma-tp1 --location northeurope --image almalinux:almalinux-x86_64:10-gen2:10.1.202512150 --size Standard_B2s_v2 --admin-username fatma --ssh-key-values "$(cat ~/.ssh/cloud_tp1.pub)"
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
Consider upgrading security for your workloads using Azure Trusted Launch VMs. To know more about Trusted Launch, please visit https://aka.ms/TrustedLaunch.
{
  "fqdns": "",
  "id": "/subscriptions/c90840e3-08fd-4a7f-bdbd-231cc7b1a045/resourceGroups/parc-info-cloud/providers/Microsoft.Compute/virtualMachines/vm-alma-tp1",
  "location": "northeurope",
  "macAddress": "00-22-48-A3-1D-93",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "128.251.0.162",
  "resourceGroup": "parc-info-cloud"
}
```

🌞 **Assurez-vous que vous pouvez vous connecter à la VM en SSH sur son IP publique**

```bash 
atma@ubuntu:~$ ssh fatma@128.251.0.162
The authenticity of host '128.251.0.162 (128.251.0.162)' can't be established.
ED25519 key fingerprint is SHA256:uNt8yaUzIku/wMhhEC+8nCyUU2Glh4qOqIyNdKhV2k8.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '128.251.0.162' (ED25519) to the list of known hosts.
[fatma@vm-alma-tp1 ~]$ 
```

🌞 **Une fois connecté, prouvez la présence...**

```bash 
[fatma@vm-alma-tp1 ~]$ systemctl status waagent.service
● waagent.service - Azure Linux Agent
     Loaded: loaded (/usr/lib/systemd/system/waagent.service; enabled; preset: >
     Active: active (running) since Tue 2026-03-24 14:12:23 UTC; 4min 7s ago
 Invocation: 3d62046cb4554f218dab4269644234bc
   Main PID: 1320 (python3)
      Tasks: 6 (limit: 50299)
     Memory: 48.4M (peak: 50.4M)
        CPU: 1.404s
     CGroup: /azure.slice/waagent.service
             ├─1320 /usr/bin/python3 -u /usr/sbin/waagent -daemon
             └─1452 /usr/bin/python3 -u bin/WALinuxAgent-2.15.1.3-py3.12.egg -r>

Mar 24 14:12:29 vm-alma-tp1 python3[1452]:     pkts      bytes target     prot >
Mar 24 14:12:29 vm-alma-tp1 python3[1452]:        0        0 ACCEPT     tcp  -->
Mar 24 14:12:29 vm-alma-tp1 python3[1452]:      117    14178 ACCEPT     tcp  -->
Mar 24 14:12:29 vm-alma-tp1 python3[1452]:        0        0 DROP       tcp  -->
Mar 24 14:12:29 vm-alma-tp1 python3[1452]: 2026-03-24T14:12:29.743254Z INFO Ext>
Mar 24 14:12:29 vm-alma-tp1 python3[1452]: 2026-03-24T14:12:29.743335Z INFO Ext>
Mar 24 14:12:29 vm-alma-tp1 python3[1452]: 2026-03-24T14:12:29.743634Z INFO Ext>
Mar 24 14:12:29 vm-alma-tp1 python3[1452]: 2026-03-24T14:12:29.744038Z INFO Ext>
Mar 24 14:12:29 vm-alma-tp1 python3[1452]: 2026-03-24T14:12:29.763204Z INFO Ext>
Mar 24 14:12:29 vm-alma-tp1 python3[1452]: 2026-03-24T14:12:29.765688Z INFO Ext>
lines 1-22/22 (END)
```

```bash 
[fatma@vm-alma-tp1 ~]$ cloud-init status
status: done
```

## 3. Terraforming ~~planets~~ infrastructures

🌞 **Utilisez Terraform pour créer une VM dans Azure**

```bash 
mkdir ~/terraform-azure && cd ~/terraform-azure
```

```bash
fatma@ubuntu:~/terraform-azure$ terraform init
```

```bash 
fatma@ubuntu:~/terraform-azure$ terraform plan

Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_linux_virtual_machine.vm will be created
  + resource "azurerm_linux_virtual_machine" "vm" {
      + admin_username                                         = "fatma"
      + allow_extension_operations                             = true
      + bypass_platform_safety_checks_on_user_schedule_enabled = false
      + computer_name                                          = (known after apply)
      + disable_password_authentication                        = true
      + disk_controller_type                                   = (known after apply)
      + extensions_time_budget                                 = "PT1H30M"
      + id                                                     = (known after apply)
      + location                                               = "northeurope"
      + max_bid_price                                          = -1
      + name                                                   = "vm-terraform-alma"
      + network_interface_ids                                  = (known after apply)
      + patch_assessment_mode                                  = "ImageDefault"
      + patch_mode                                             = "ImageDefault"
      + platform_fault_domain                                  = -1
      + priority                                               = "Regular"
      + private_ip_address                                     = (known after apply)
      + private_ip_addresses                                   = (known after apply)
      + provision_vm_agent                                     = true
      + public_ip_address                                      = (known after apply)
      + public_ip_addresses                                    = (known after apply)
      + resource_group_name                                    = "rg-terraform-tp"
      + size                                                   = "Standard_B2s_v2"
      + virtual_machine_id                                     = (known after apply)
      + vm_agent_platform_updates_enabled                      = false

      + admin_ssh_key {
          + public_key = <<-EOT
                ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC//6eBGc6xRHvTaNkYM0AtJzmmN/VxyqFNcfrvfCER8E1wcWSccmnL9Otn5N3yrk3kb/Yv/7caMMub583iGaw3SoT4WhtLeBCGPgesn1DiXfAZisffkq1bt2Zk/yMA5y72CACqEYZkO+LcQfAaMevPUwcTzdptwW5dWzOXKJDR9EyawF4eumKOHRxjIJ0sd8QiOF5QlIzCN9HSC/7Wd6XMIlVqTCEE+G2bM4HVK1ik6mgBbVRKXK902KDOw5U8yH0eqzNyjRVkhVC6w10mvbrrXhxSYNI8AZw8nZasrJQ9ZzOLd32vyQUHUvUXsHkbZD35xy27slUj8LpjmQQGH9LQeCGqcjohro6slvWA2UbQ5nDXPEzrqkWLJ6YuslmUeJcfjhGXCC1HTGHBliVYMH9v44OSj49z7xqqg4Mm2NrqWx6IxGVbsK8ZtpM7f7Rf9plnc5uWRt2bFkebfLrDAObF0lId7pY1FvmBffDZZVSD53GdZuT/S6FGtmhc5Rhdu4vV7ezBz2uIRAhEI1FdyoLQoAiPxy1QiXMfR6dh89uyHo3UuibZ9tDH9Boll+izBP88in2pIFpsEdveQwrbix5UxFGcE+Nz5h9ik76EmWRLaQGR58ipiK4IKOdUDaLLKIhbCbR5iSWnK4fIyqW8/yZy0Y9k8VCzRRzWYwRbn9lT+w== fatma@ubuntu
            EOT
          + username   = "fatma"
        }

      + os_disk {
          + caching                   = "ReadWrite"
          + disk_size_gb              = (known after apply)
          + name                      = (known after apply)
          + storage_account_type      = "Standard_LRS"
          + write_accelerator_enabled = false
        }

      + source_image_reference {
          + offer     = "almalinux-x86_64"
          + publisher = "almalinux"
          + sku       = "10-gen2"
          + version   = "latest"
        }

      + termination_notification (known after apply)
    }

  # azurerm_network_interface.nic will be created
  + resource "azurerm_network_interface" "nic" {
      + accelerated_networking_enabled = (known after apply)
      + applied_dns_servers            = (known after apply)
      + dns_servers                    = (known after apply)
      + enable_accelerated_networking  = (known after apply)
      + enable_ip_forwarding           = (known after apply)
      + id                             = (known after apply)
      + internal_domain_name_suffix    = (known after apply)
      + ip_forwarding_enabled          = (known after apply)
      + location                       = "northeurope"
      + mac_address                    = (known after apply)
      + name                           = "nic-tp"
      + private_ip_address             = (known after apply)
      + private_ip_addresses           = (known after apply)
      + resource_group_name            = "rg-terraform-tp"
      + virtual_machine_id             = (known after apply)

      + ip_configuration {
          + gateway_load_balancer_frontend_ip_configuration_id = (known after apply)
          + name                                               = "internal"
          + primary                                            = (known after apply)
          + private_ip_address                                 = (known after apply)
          + private_ip_address_allocation                      = "Dynamic"
          + private_ip_address_version                         = "IPv4"
          + public_ip_address_id                               = (known after apply)
          + subnet_id                                          = (known after apply)
        }
    }

  # azurerm_public_ip.pip will be created
  + resource "azurerm_public_ip" "pip" {
      + allocation_method       = "Static"
      + ddos_protection_mode    = "VirtualNetworkInherited"
      + fqdn                    = (known after apply)
      + id                      = (known after apply)
      + idle_timeout_in_minutes = 4
      + ip_address              = (known after apply)
      + ip_version              = "IPv4"
      + location                = "northeurope"
      + name                    = "pip-tp"
      + resource_group_name     = "rg-terraform-tp"
      + sku                     = "Basic"
      + sku_tier                = "Regional"
    }

  # azurerm_resource_group.rg will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + location = "northeurope"
      + name     = "rg-terraform-tp"
    }

  # azurerm_subnet.subnet will be created
  + resource "azurerm_subnet" "subnet" {
      + address_prefixes                               = [
          + "10.0.1.0/24",
        ]
      + default_outbound_access_enabled                = true
      + enforce_private_link_endpoint_network_policies = (known after apply)
      + enforce_private_link_service_network_policies  = (known after apply)
      + id                                             = (known after apply)
      + name                                           = "subnet-tp"
      + private_endpoint_network_policies              = (known after apply)
      + private_endpoint_network_policies_enabled      = (known after apply)
      + private_link_service_network_policies_enabled  = (known after apply)
      + resource_group_name                            = "rg-terraform-tp"
      + virtual_network_name                           = "vnet-tp"
    }

  # azurerm_virtual_network.vnet will be created
  + resource "azurerm_virtual_network" "vnet" {
      + address_space       = [
          + "10.0.0.0/16",
        ]
      + dns_servers         = (known after apply)
      + guid                = (known after apply)
      + id                  = (known after apply)
      + location            = "northeurope"
      + name                = "vnet-tp"
      + resource_group_name = "rg-terraform-tp"
      + subnet              = (known after apply)
    }

Plan: 6 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + public_ip = (known after apply)

───────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't
guarantee to take exactly these actions if you run "terraform apply" now.
```





🌞 **Prouvez avec une connexion SSH sur l'IP publique que la VM est up**

```bash 

```
