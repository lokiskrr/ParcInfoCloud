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

```

## 2. Tester le template

🌞 **Lancer une VM à partir de votre template**

``` bash 

```

🌞 **Vérification !**

``` bash 

```

# TP3B Part4 - Hardened template

🌞 **Créez une VM qui servira à créer le template**

``` bash 

```

# TP3B Part4 - F. Templante then Deploy

## 1. Create the template

🌞 **Clean la VM**

``` bash 

```

🌞 **Faire de la VM un template**

``` bash 

```

## 2. Test

🌞 **Lancer une VM à partir de cette image**

``` bash 

```

🌞 **Vérif**

``` bash 

```