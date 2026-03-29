# TP3B : Hardened base image

## 1. Intro

## 2. Feu patate

🌞 **Créez une VM azure** (une commande `az`)

``` bash 
az vm create   --name vm1   --resource-group tp1-ne   --location denmarkeast   --image almalinux:almalinux-x86_64:9-gen2:latest   --admin-username azureuser   --generate-ssh-keys   --size Standard_B2s_v2   --public-ip-sku Standard
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
Consider upgrading security for your workloads using Azure Trusted Launch VMs. To know more about Trusted Launch, please visit https://aka.ms/TrustedLaunch.
{
  "fqdns": "",
  "id": "/subscriptions/c90840e3-08fd-4a7f-bdbd-231cc7b1a045/resourceGroups/tp1-ne/providers/Microsoft.Compute/virtualMachines/vm1",
  "location": "denmarkeast",
  "macAddress": "7C-ED-8D-37-C9-2A",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "9.205.155.115",
  "resourceGroup": "tp1-ne"
}
```

🌞 **Connexion SSH**

``` bash 
fatma@ubuntu:~$ ssh -i ~/.ssh/cloud_tp1 azureuser@9.205.155.115
[azureuser@vm1 ~]$ 
```

# TP3B Part2 - Prepare the VM

## 1. Poser notre conf custom

🌞 **Effectuez la conf suivante :**

``` bash 
[azureuser@vm1 ~]$ sudo dnf update -y
[azureuser@vm1 ~]$ sudo dnf install -y epel-release
[azureuser@vm1 ~]$ sudo dnf makecache
[azureuser@vm1 ~]$ sudo dnf install -y htop vim bind-utils iputils
```

```bash
[azureuser@vm1 ~]$ htop --version
htop 3.3.0
[azureuser@vm1 ~]$ vim --version
VIM - Vi IMproved 8.2 (2019 Dec 12, compiled Sep 17 2025 00:00:00)
Included patches: 1-2637
Modified by <bugzilla@redhat.com>
Compiled by <bugzilla@redhat.com>
Huge version without GUI.  Features included (+) or not (-):
+acl               -farsi             +mouse_sgr         +tag_binary
+arabic            +file_in_path      -mouse_sysmouse    -tag_old_static
+autocmd           +find_in_path      +mouse_urxvt       -tag_any_white
+autochdir         +float             +mouse_xterm       -tcl
-autoservername    +folding           +multi_byte        +termguicolors
-balloon_eval      -footer            +multi_lang        +terminal
+balloon_eval_term +fork()            -mzscheme          +terminfo
-browse            +gettext           +netbeans_intg     +termresponse
++builtin_terms    -hangul_input      +num64             +textobjects
+byte_offset       +iconv             +packages          +textprop
+channel           +insert_expand     +path_extra        +timers
+cindent           +ipv6              +perl/dyn          +title
-clientserver      +job               +persistent_undo   -toolbar
-clipboard         +jumplist          +popupwin          +user_commands
+cmdline_compl     +keymap            +postscript        +vartabs
+cmdline_hist      +lambda            +printer           +vertsplit
+cmdline_info      +langmap           +profile           +virtualedit
+comments          +libcall           -python            +visual
+conceal           +linebreak         +python3/dyn       +visualextra
+cryptv            +lispindent        +quickfix          +viminfo
+cscope            +listcmds          +reltime           +vreplace
+cursorbind        +localmap          +rightleft         +wildignore
+cursorshape       +lua/dyn           +ruby/dyn          +wildmenu
+dialog_con        +menu              +scrollbind        +windows
+diff              +mksession         +signs             +writebackup
+digraphs          +modify_fname      +smartindent       -X11
-dnd               +mouse             -sound             -xfontset
-ebcdic            -mouseshape        +spell             -xim
+emacs_tags        +mouse_dec         +startuptime       -xpm
+eval              +mouse_gpm         +statusline        -xsmp
+ex_extra          -mouse_jsbterm     -sun_workshop      -xterm_clipboard
+extra_search      +mouse_netterm     +syntax            -xterm_save
   system vimrc file: "/etc/vimrc"
     user vimrc file: "$HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
       defaults file: "$VIMRUNTIME/defaults.vim"
  fall-back for $VIM: "/etc"
 f-b for $VIMRUNTIME: "/usr/share/vim/vim82"
Compilation: gcc -c -I. -Iproto -DHAVE_CONFIG_H -O2 -flto=auto -ffat-lto-objects -fexceptions -g -grecord-gcc-switches -pipe -Wall -Werror=format-security -Wp,-D_GLIBCXX_ASSERTIONS -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -fstack-protector-strong -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -m64 -march=x86-64-v2 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -D_REENTRANT -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1 
Linking: gcc -L. -Wl,-z,relro -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -fstack-protector-strong -rdynamic -Wl,-export-dynamic -Wl,--no-as-needed -Wl,--enable-new-dtags -Wl,-z,relro -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -Wl,-z,relro -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -L/usr/local/lib -Wl,--as-needed -o vim -lm -lselinux -lncurses -lacl -lattr -lgpm -Wl,--enable-new-dtags -Wl,-z,relro -Wl,--as-needed -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -Wl,-z,relro -Wl,--as-needed -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -fstack-protector-strong -L/usr/local/lib -L/usr/lib64/perl5/CORE -lperl -lpthread -lresolv -ldl -lm -lcrypt -lutil -lc 
[azureuser@vm1 ~]$ dig -v
DiG 9.16.23-RH

```

## 2. Clean la VM

### A. Réinitiliser `cloud-init`

🌞 **Go lancer ça :**

``` bash 
[azureuser@vm1 ~]$ sudo cloud-init clean --logs
[azureuser@vm1 ~]$ sudo rm -rf /var/log/*
[azureuser@vm1 ~]$ sudo systemctl enable cloud-init
```

### B. Clean le système

🌞 **Proposer une suite de commandes**

``` bash 
[azureuser@vm1 ~]$ sudo rm -rf /var/log/*
[azureuser@vm1 ~]$ sudo dnf clean all
[azureuser@vm1 ~]$ sudo rm -rf /tmp/* /var/tmp/*
[azureuser@vm1 ~]$ history -c && > ~/.bash_history
```

# TP3B Part3 - Create a template

## 1. Créer le template

🌞 **Let's go, balancez :**

``` bash 
fatma@ubuntu:~$ az vm deallocate --resource-group tp1-ne --name vm1-ne
fatma@ubuntu:~$ az vm generalize --resource-group tp1-ne --name vm1-ne
fatma@ubuntu:~$ az image create \
  --resource-group tp1-ne \
  --name alma_chad \
  --source vm1-ne \
  --hyper-v-generation V2
{
  "hyperVGeneration": "V2",
  "id": "/subscriptions/c90840e3-08fd-4a7f-bdbd-231cc7b1a045/resourceGroups/tp1-ne/providers/Microsoft.Compute/images/alma_chad",
  "location": "northeurope",
  "name": "alma_chad",
  "provisioningState": "Succeeded",
  "resourceGroup": "tp1-ne",
  "sourceVirtualMachine": {
    "id": "/subscriptions/c90840e3-08fd-4a7f-bdbd-231cc7b1a045/resourceGroups/tp1-ne/providers/Microsoft.Compute/virtualMachines/vm1-ne",
    "resourceGroup": "tp1-ne"
  },
  "storageProfile": {
    "dataDisks": [],
    "osDisk": {
      "caching": "ReadWrite",
      "diskSizeGB": 30,
      "managedDisk": {
        "id": "/subscriptions/c90840e3-08fd-4a7f-bdbd-231cc7b1a045/resourceGroups/tp1-ne/providers/Microsoft.Compute/disks/vm1-ne_OsDisk_1_3e679bc2a6b44fa880cda0a6f74a8247",
        "resourceGroup": "tp1-ne"
      },
      "osState": "Generalized",
      "osType": "Linux",
      "storageAccountType": "Premium_LRS"
    }
  },
  "tags": {},
  "type": "Microsoft.Compute/images"
}
```

## 2. Tester le template

🌞 **Lancer une VM à partir de votre template**

``` bash 
fatma@ubuntu:~$ az vm create   --name vm-test   --resource-group tp1-ne   --location northeurope   --image alma_chad   --admin-username azureuser   --ssh-key-values "$(cat ~/.ssh/cloud_tp1.pub)"   --size Standard_B2s_v2   --public-ip-sku Standard
The default value of '--size' will be changed to 'Standard_D2s_v5' from 'Standard_DS1_v2' in a future release.
{
  "fqdns": "",
  "id": "/subscriptions/c90840e3-08fd-4a7f-bdbd-231cc7b1a045/resourceGroups/tp1-ne/providers/Microsoft.Compute/virtualMachines/vm-test",
  "location": "northeurope",
  "macAddress": "70-A8-A5-2F-E2-03",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "68.219.215.18",
  "resourceGroup": "tp1-ne"
}
```

🌞 **Vérification !**

``` bash 
fatma@ubuntu:~$ ssh -i ~/.ssh/cloud_tp1 azureuser@68.219.215.18
The authenticity of host '68.219.215.18 (68.219.215.18)' can't be established.
ED25519 key fingerprint is SHA256:JN0QtbBfqnWnXiwigszKBsRtLsJXWTCBR/0nDsmHVoY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '68.219.215.18' (ED25519) to the list of known hosts.
[azureuser@vm-test ~]$ htop --version
htop 3.3.0
[azureuser@vm-test ~]$ cloud-init status
status: done
[azureuser@vm-test ~]$ systemctl status waagent
● waagent.service - Azure Linux Agent
     Loaded: loaded (/usr/lib/systemd/system/waagent.service; enabled; preset: >
     Active: active (running) since Tue 2026-03-24 17:59:06 UTC; 2min 55s ago
   Main PID: 1297 (python3)
      Tasks: 6 (limit: 48588)
     Memory: 40.3M (peak: 46.9M)
        CPU: 1.117s
     CGroup: /azure.slice/waagent.service
             ├─1297 /usr/bin/python3 -u /usr/sbin/waagent -daemon
             └─1522 /usr/bin/python3 -u bin/WALinuxAgent-2.15.1.3-py3.12.egg -r>

Mar 24 17:59:08 vm-test python3[1522]:     pkts      bytes target     prot opt >
Mar 24 17:59:08 vm-test python3[1522]: Chain FORWARD (policy ACCEPT 0 packets, >
Mar 24 17:59:08 vm-test python3[1522]:     pkts      bytes target     prot opt >
Mar 24 17:59:08 vm-test python3[1522]: Chain OUTPUT (policy ACCEPT 122 packets,>
Mar 24 17:59:08 vm-test python3[1522]:     pkts      bytes target     prot opt >
Mar 24 17:59:08 vm-test python3[1522]:        0        0 ACCEPT     tcp  --  * >
Mar 24 17:59:08 vm-test python3[1522]:      158    23327 ACCEPT     tcp  --  * >
Mar 24 17:59:08 vm-test python3[1522]:        0        0 DROP       tcp  --  * >
Mar 24 17:59:08 vm-test python3[1522]: 2026-03-24T17:59:08.553904Z INFO EnvHand>
Mar 24 17:59:08 vm-test python3[1522]: 2026-03-24T17:59:08.568830Z INFO ExtHand>
lines 1-21/21 (END)
```

# TP3B Part4 - Hardened template

🌞 **Créez une VM qui servira à créer le template**

```bash 
fatma@ubuntu:~$ az vm create \
  --name vm-harden \
  --resource-group tp1-ne \
  --location denmarkeast \
  --image almalinux:almalinux-x86_64:9-gen2:latest \
  --admin-username azureuser \
  --generate-ssh-keys \
  --size Standard_B1s \
  --public-ip-sku Standard
```

# TP3B Part4 - A. Firewalling baby

🌞 **Firewall conf**

```bash 
[azureuser@vm-harden ~]$ sudo systemctl status firewalld
Unit firewalld.service could not be found.
[azureuser@vm-harden ~]$ sudo dnf install firewalld -y
[azureuser@vm-harden ~]$ sudo systemctl enable --now firewalld
[azureuser@vm-harden ~]$ sudo systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset>
     Active: active (running) since Sun 2026-03-29 14:51:25 UTC; 9s ago
       Docs: man:firewalld(1)
   Main PID: 2128 (firewalld)
      Tasks: 2 (limit: 5160)
     Memory: 25.3M (peak: 27.3M)
        CPU: 308ms
     CGroup: /system.slice/firewalld.service
             └─2128 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid

Mar 29 14:51:24 vm-harden systemd[1]: Starting firewalld - dynamic firewall dae>
Mar 29 14:51:25 vm-harden systemd[1]: Started firewalld - dynamic firewall daem>
lines 1-13/13 (END)

[azureuser@vm-harden ~]$ sudo firewall-cmd --list-services
cockpit dhcpv6-client ssh

[azureuser@vm-harden ~]$ sudo firewall-cmd --permanent --add-port=22/tcp
success

[azureuser@vm-harden ~]$ sudo firewall-cmd --reload
success

[azureuser@vm-harden ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources: 
  services: cockpit
  ports: 22/tcp
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

# TP3B Part4 - B. Stronk SSH

🌞 **Proposez une conf OpenSSH forte**

```bash 
[azureuser@vm-harden ~]$ sudo bash -c 'cat > /etc/ssh/sshd_config << "EOF"
> # /etc/ssh/sshd_config

Port 22
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
Protocol 2
MaxAuthTries 3
LoginGraceTime 30
LogLevel VERBOSE
AllowUsers azureuser
X11Forwarding no
AllowTcpForwarding no
EOF'
```

⭐ **BONUS** :

```bash 
i will do it at the end (maybe)
```

# TP3B Part4 - C. fail2ban

🌞 **Installer et configurer `fail2ban`**

```bash 
[azureuser@vm-harden ~]$ sudo dnf install -y epel-release

[azureuser@vm-harden ~]$ sudo dnf install -y fail2ban

[azureuser@vm-harden ~]$ fail2ban-client --version
Fail2Ban v1.1.0

sudo bash -c 'cat > /etc/fail2ban/jail.local << "EOF"
[sshd]
enabled = true
port = 22
filter = sshd
logpath = /var/log/secure
maxretry = 2
findtime = 3600
bantime = -1
EOF'

[azureuser@vm-harden ~]$ sudo systemctl restart fail2ban
[azureuser@vm-harden ~]$ sudo systemctl status fail2ban
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/usr/lib/systemd/system/fail2ban.service; disabled; preset>
     Active: active (running) since Sun 2026-03-29 15:16:57 UTC; 6s ago
       Docs: man:fail2ban(1)
    Process: 3533 ExecStartPre=/bin/mkdir -p /run/fail2ban (code=exited, status>
   Main PID: 3534 (fail2ban-server)
      Tasks: 5 (limit: 5160)
     Memory: 12.2M (peak: 12.6M)
        CPU: 133ms
     CGroup: /system.slice/fail2ban.service
             └─3534 /usr/bin/python3 -s /usr/bin/fail2ban-server -xf start

Mar 29 15:16:57 vm-harden systemd[1]: Starting Fail2Ban Service...
Mar 29 15:16:57 vm-harden systemd[1]: Started Fail2Ban Service.
Mar 29 15:16:57 vm-harden fail2ban-server[3534]: Server ready
lines 1-15/15 (END)

[azureuser@vm-harden ~]$ 
[azureuser@vm-harden ~]$ sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed:	2
|  |- Total failed:	2
|  `- Journal matches:	_SYSTEMD_UNIT=sshd.service + _COMM=sshd + _COMM=sshd-session
`- Actions
   |- Currently banned:	0
   |- Total banned:	0
   `- Banned IP list:	
```

🌞 **Prouvez que `fail2ban` fonctionne**

```bash 
fatma@ubuntu:~$ ssh -i ~/.ssh/cle_test azureuser@9.205.153.216
azureuser@9.205.153.216: Permission denied (publickey).
fatma@ubuntu:~$ ssh -i ~/.ssh/cle_test azureuser@9.205.153.216
azureuser@9.205.153.216: Permission denied (publickey).


fatma@ubuntu:~$ ssh -i ~/.ssh/mauvaise_cle azureuser@9.205.153.216
Last login: Sun Mar 29 15:50:55 2026 from 88.173.243.32

[azureuser@vm-harden ~]$ sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 3
|  |- Total failed:     3
|  `- Journal matches:  _SYSTEMD_UNIT=sshd.service + _COMM=sshd + _COMM=sshd-session
`- Actions
   |- Currently banned: 1
   |- Total banned:     1
   `- Banned IP list:   8...2
```

# TP3B Part4 - D. Harden kernel parameters

## 1. Intro

## 2. Setup

```bash 

Modification : 

net.ipv4.ip_forward = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1



[azureuser@vm-harden ~]$ sudo nano /etc/sysctl.d/99-harden.conf
[azureuser@vm-harden ~]$ sudo sysctl --system
* Applying /usr/lib/sysctl.d/10-default-yama-scope.conf ...
* Applying /usr/lib/sysctl.d/50-coredump.conf ...
* Applying /usr/lib/sysctl.d/50-default.conf ...
* Applying /usr/lib/sysctl.d/50-libkcapi-optmem_max.conf ...
* Applying /usr/lib/sysctl.d/50-pid-max.conf ...
* Applying /usr/lib/sysctl.d/50-redhat.conf ...
* Applying /etc/sysctl.d/99-harden.conf ...
* Applying /etc/sysctl.d/99-sysctl.conf ...
* Applying /etc/sysctl.conf ...
kernel.yama.ptrace_scope = 0
kernel.core_pattern = |/usr/lib/systemd/systemd-coredump %P %u %g %s %t %c %h %d %F
kernel.core_pipe_limit = 16
fs.suid_dumpable = 2
kernel.sysrq = 16
kernel.core_uses_pid = 1
net.ipv4.conf.default.rp_filter = 2
net.ipv4.conf.eth0.rp_filter = 2
net.ipv4.conf.lo.rp_filter = 2
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.eth0.accept_source_route = 0
net.ipv4.conf.lo.accept_source_route = 0
net.ipv4.conf.default.promote_secondaries = 1
net.ipv4.conf.eth0.promote_secondaries = 1
net.ipv4.conf.lo.promote_secondaries = 1
net.ipv4.ping_group_range = 0 2147483647
net.core.default_qdisc = fq_codel
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
fs.protected_regular = 1
fs.protected_fifos = 1
net.core.optmem_max = 81920
kernel.pid_max = 4194304
kernel.kptr_restrict = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.eth0.rp_filter = 1
net.ipv4.conf.lo.rp_filter = 1
net.ipv4.ip_forward = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
```

# TP3B Part4 - E. IDS

## 1. Intro

## 2. Setup simple

🌞 **Installer l'IDS AIDE**

```bash 
[azureuser@vm-harden ~]$ sudo dnf install aide -y
```

🌞 **Proposer une conf AIDE**

```bash 
sudo nano /etc/aide.conf

/etc/ssh/sshd_config
/etc/sysctl.d/99-harden.conf

[azureuser@vm-harden ~]$ sudo aide --init
Start timestamp: 2026-03-29 16:14:53 +0000 (AIDE 0.16)
AIDE initialized database at /var/lib/aide/aide.db.new.gz

Number of entries:	33048

---------------------------------------------------
The attributes of the (uncompressed) database(s):
---------------------------------------------------

/var/lib/aide/aide.db.new.gz
  MD5      : QsWzcob8GBdGuG1oRTcGqA==
  SHA1     : +YBdjpet3K3klrpKm8ImOkRAm9s=
  RMD160   : cGrnMXAQlO5ixGi13G0wNTswMiU=
  TIGER    : 2tb2DOCZFlU36q655wdzYrKlO/RGoFj0
  SHA256   : CVf6ej9+ZPfrpvBc/e+vwe3Pmjb4eHKZ
             jO0KMys0UaA=
  SHA512   : cmBe4SfQFS6EkKAO9xKOFZZtlDyfTV/Q
             44dDJwfasjVkpjhi2+V37b0T4cTuKSwM
             g35yvXBOyxYPdmFjImTueA==


End timestamp: 2026-03-29 16:15:19 +0000 (run time: 0m 26s)



```

🌞 **Initialiser la base de données AIDE**

```bash 

```

🌞 **Jouer avec les tests d'intégrité AIDE**

```bash 

```

## 3. Automated

🌞 **Créer un service systemd pour lancer un test AIDE**

```bash 

```
- configuration simple et sécurisée
🌞 **Indiquer à systemd qu'on a modifié les services**

```bash 

```

🌞 **Tester le service**

```bash 

```

🌞 **Créer un *timer* systemd**

```bash 

```

## 4. Bonus : Alerte Discord

⭐ **Proposer un setup qui permet de recevoir des alertes Discord**

```bash 

```

# TP3B Part4 - F. Templante then Deploy

## 1. Create the template

🌞 **Clean la VM**

```bash 

```

🌞 **Faire de la VM un template**

```bash 

```

## 2. Test

🌞 **Lancer une VM à partir de cette image**

```bash 

```

🌞 **Vérif**

```bash 

```