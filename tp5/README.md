# PROJET RESEAU - TP5
## Premier pas dans le monde Cisco

### I. Préparation du lab

#### 1. Préparation VMs

>TP produit sous l'OS Fedora 29.

##### A. Création d'un nouveau host-only
Pour se faire, j'ouvre VirtualBox et fait défiler le menu déroulant `Fichier` et accèder à `Gestionnaire de réseau hôte`. Enfin je coche la case `Activé` pou le serveur DHCP.
##### B. Création des VMs
Pour chaque machines, j'ouvre le menu `Configuration` -> `Réseau`afin d'activer les deux premières interfaces.
Enfin, pour chaque machine, j'accède à leur fichier configuration de leur carte NAT. Chemin à suivre `$ cd /etc/sysconfig/network-script` puis `$ sudo nano ifcfg-enp0s3`. Je rajoute la ligne `IPPADR=` avec l'IP demandée selon la machine ainsi que leur `NETMASK=255.255.255.0`
##### C. Clone des VMs
Dans GNS3, on ajoute les trois VMs dans le menu `Edit > Préférences > VirtualBox VMs > Add`
##### D. Configuration réseau des machines dans GNS3
Dans le même menu que précédemment, j'accède à `Edit > Network` pour chaque machines et je rentre la valeur `2` dans `Adapters`

#### 2. Préparation Routeurs Cisco
Pour configurer les routeurs, il faut lancer la console de GNS3 en double-cliquant sur les machines routers.
Nous avons un invité de commande afin de définir une IP statique aux ports de nos routeurs.
```
# show ip int br

```
