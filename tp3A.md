# TP3A : Terraform + Azure

# I. Network Security Group

## 1. Ptite intro

## 2. Ajouter un NSG au déploiement

🌞 **[Ajouter un NSG à votre déploiement Terraform](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/network_security_group)**

``` bash 

```

## 3. Proofs !

🌞 **Prouver que ça fonctionne, rendu attendu :**

``` bash 

```

📁 **Fichiers attendus**

# II. Un ptit nom DNS

## 1. Adapter le plan Terraform

🌞 **Donner un nom DNS à votre VM**

``` bash 

```

## 2. Ajouter un output custom à `terraform apply`

🌞 **Un ptit output nan ?**

``` bash 

```

## 3. Proooofs ! 

🌞 **Proofs ! Donnez moi :**

``` bash 

```

📁 **Fichiers attendus**

# III. Blob storage

## 1. Intro

## 2. Let's go

🌞 **Compléter votre plan Terraform pour déployer du Blob Storage pour votre VM**

``` bash 

```

📁 **Fichiers attendus**

## 3. Proooooooofs

🌞 **Prouvez que tout est bien configuré, depuis la VM Azure**

``` bash 

```

🌞 **Déterminez comment `azcopy login --identity` vous a authentifié**

``` bash 

```

🌞 **Requêtez un JWT d'authentification auprès du service que vous venez d'identifier, manuellement**

``` bash 

```

🌞 **Expliquez comment l'IP `169.254.169.254` peut être joignable**

``` bash 

```

# IV. Monitoring

## 1. Introw

## 2. Une alerte CPU

🌞 **Compléter votre plan Terraform et mettez en place une alerte CPU**

``` bash 

```

📁 **Fichiers attendus**

## 3. Alerte mémoire

🌞 **Compléter votre plan Terraform et mettez en place une alerte mémoire**

``` bash 

```

📁 **Fichiers attendus**

## 4. Proofs

### A. Voir les alertes avec `az`

🌞 **Une commande `az` qui permet de lister les alertes actuellement configurées**

``` bash 

```

### B. Stress pour *fire* les alertes

🌞 **Stress de la machine**

``` bash 

```

🌞 **Vérifier que des alertes ont été *fired***

``` bash 

```

# V. Azure Vault

## 1. Lil' intro ?

## 2. Do it !

🌞 **Compléter votre plan Terraform et mettez en place une *Azure Key Vault***

``` bash 

```

📁 **Fichiers attendus**

## 3. Proof proof proof

🌞 **Avec une commande `az`, afficher le *secret***

``` bash 

```

🌞 **Depuis la VM, afficher le secret**

``` bash 

```