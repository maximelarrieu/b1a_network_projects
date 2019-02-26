# PROJET RESEAU - TP6
## Une topologie qui ressemble un peu à quelque chose, enfin ?
### I. Mise en place du lab
#### Checklist IP Routeurs
* Pour définir les IP statiques des routeurs, je procède comme nous l'avions pu faire précédemment. L'exemple utilisé représente la mise en place de mon `router1`. Elle sera la même pour tous les autres, les IPs à leur attribuer ne seront évidemment pas les même :
```
R1# conf t
R1(config)# interface eth0/0
R1(config-if)# ip address 10.6.100.1 255.255.255.252
R1(config-if)# no shut
R1(config-if)# exit
R1(config)# interface eth0/1
R1(config-if)# ip address 10.6.100.5 255.255.255.252
R1(config-if)# no shut
R1(config-if)# exit
R1(config)# interface eth0/2
R1(config-if)# ip address 10.6.201.254 255.255.255.252
R1(config-if)# no shut
R1(config-if)# exit
R1(config)# exit
```
Je peux maintenant voir les IPs attribuées aux interfaces :
```
R1# show ip int br
Interface			IP-Address		Ok? Method status		Protocol
Ethernet0/0			10.6.100.1		Yes Manual up			up
Ethernet0/1			10.6.100.5		Yes Manual up			up
Ethernet0/2			10.6.201.254	Yes Manual up			up
Ethernet0/3			unassigned		Yes Manual up			up
```
* Pour définir leurs noms de domaine, j'utilise :
```
R1# conf t
R1(config)# hostname r1.tp5.b1
```
#### Checklist VMs
* Pour définir leurs IPs statiques, je reprends la procédure déjà faite dans plusieurs tp. L'exemple utilisé permet de configurer ma machine `client1`.
Je dois aller modifier le fichier de l'interface et y modifier quelques lignes :
```
$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3

BOOTPROTO="static"
NAME="enp0s3"
DEVICE="enp0s3"
ONBOOT="yes"
IPADDR=10.6.201.10
NETMASK=255.255.255.252
```
Et on relance l'interface :
```
$ sudo ifdown enp0s3
$ sudo ifup enp0s3
```
* Pour modifier leurs noms de domaine, on utilise la commande : `$ echo 'client1.tp6.b1 | sudo tee /etc/hostname'`
* Enfin, on vient remplir les fichiers hosts :
```
$ sudo nano /etc/hosts

10.6.201.11 client2.tp6.b1
10.6.202.10 server1.tp6.b1
```
#### Vérifier que tout ça fonctionne
Le ping fonctionne !

#### Configuration OSPF
+ Ici pour l'exemple, je configure pour `router1`. Pour activer OSPF, je procède comme ceci :
```
R1# conf t
R1(config)# router ospf 1
```
+ Pour configurer l'`id-router`, je procède comme suit :
`R1(config-router)# router-id 1.1.1.1`
+ Maintenant, je configure les routes à partager, je suis la procédure :
```
R1(config-router)# network 10.6.100.0 0.0.0.3 area 0
R1(config-router)# network 10.6.100.4 0.0.0.3 area 0
R1(config-router)# network 10.6.202.0 0.0.0.3 area 2
```
```
R2(config-router)# network 10.6.100.0 0.0.0.3 area 0
R2(config-router)# network 10.6.100.4 0.0.0.3 area 0
R2(config-router)# network 10.6.202.0 0.0.0.3 area 2
```
