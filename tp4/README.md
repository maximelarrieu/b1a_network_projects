# PROJET RESEAU - TP4
## SPELEOLOGIE RESEAU : DESCENTE DANS LES COUCHES

### I. Mise en place du lab
#### 1. Création des réseaux
Pour créer de nouveaux réseaux host-only, je suis allé dans le menu "Gestionnaire de réseau hôte" dans VirtualBox. Ainsi j'ai pu créer un premier réseau portant l'IP `net1 : 10.1.0.1` et le second avec `net2 : 10.2.0.1`

#### 2. Créations des VMs
J'ai cloné ma VM patron pour en faire 3 : Client, Server, Router.
Pour leur faire porter les adresses IP demandées, je dois tout d'abord accèder au dossier network-scripts : `cd /etc/sysconfig/network-scripts/`.
Ainsi je peux ouvrir le fichier correspondant à la carte réseau : `sudo nano ifcfg-enp0s3` et modifier le contenu, notamment l'adresse IP en ajoutant la ligne `IPADDR=ip demandée`.

#### 3. Mise en place du routage statique   
Après avoir activé l'IPv4 Forwarding, je peux voir s'il y a des routes pour aller vers net1°1 et net2 :
> Sur routeur
```
$ ip route show
10.1.0.0/24 dev enp0s3 proto kernel scope link src 10.1.0.254 metric 100 
10.2.0.0/24 dev enp0s8 proto kernel scope link src 10.2.0.254 metric 101 
```
> Sur client

Je dois ajouter une route statique pour que le client puisse avoir une route vers `net1` et `net2`
```
net1
$ ip route add 10.1.0.0/24 via 10.1.0.254 dev enp0s3
$ ip route show
10.1.0.0/24 dev enp0s3 proto kernel scope link src 10.1.0.10 metric 100

net2
$ ip route add 10.2.0.0/24 via 10.1.0.254 dev enp0s3
$ ip route show
10.2.0.0/24 via 10.1.0.254 dev enp0s3
```
> Sur serveur
```
net1
$ ip route add 10.1.0.0/24 via 10.2.0.254 dev enp0s3
$ ip r s
10.1.0.0/24 via 10.2.0.254 dev enp0s3 

net2
$ ip route add 10.2.0.0/24 via 10.1.0.254 dev enp0s3
$ ip s r
10.2.0.0/24 dev enp0s3 proto kernel scope link src 10.2.0.10 metric 100
```

> Test

> Client ping Server
```
$ ping 10.2.0.10
PING 10.2.0.10 (10.2.0.10) 56(84) bytes of data.
64 bytes from 10.2.0.10: icmp_seq=1 ttl=63 time=1.47 ms
64 bytes from 10.2.0.10: icmp_seq=2 ttl=63 time=2.09 ms
...
```
>Server ping Client
```
$ ping 10.1.0.10
PING 10.1.0.10 (10.1.0.10) 56(84) bytes of data.
64 bytes from 10.1.0.10: icmp_seq=1 ttl=63 time=1.53 ms
64 bytes from 10.1.0.10: icmp_seq=2 ttl=63 time=1.83 ms
...
```

### 2. Spéléologie réseau
#### 1. ARP
##### A. Manip 1
1. Pour vider la table ARP de mes machines, j'utilise la commande : `$ sudo ip neigh flush all`
2. J'affiche ma table ARP sur Client :
```
$ ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:01 DELAY
```
...

3. J'affiche ma table ARP sur Serveur :
```
$ ip neigh show
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:02 DELAY
```
...

4. Je ping ma machine Server avec Client : `$ ping 10.2.0.10`
J'affiche de nouveau la table ARP de ma machine Client :
```
$ ip neigh show
10.1.0.1 dev enp0s3 lladdr 0a:00:27:00:00:01 REACHABLE
10.1.0.254 dev enp0s3 lladdr 08:00:27:1e:71:a1 REACHABLE
```
...

5. J'affiche de nouveau ma table ARP de ma machine Server :
```
$ ip neigh show
10.2.0.1 dev enp0s3 lladdr 0a:00:27:00:00:02 REACHABLE
10.2.0.254 dev enp0s3 lladdr 08:00:27:d7:a7:a0 STALE
```
...

##### B. Manip 2
1. Je revide les tables ARP de toutes mes machines : `$ sudo ip neigh flush all`
2. J'affiche la table ARP de ma machine Router :
```
$ sudo ip neigh show
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:02 REACHABLE
```
...

3. Avec ma machine Client je ping ma machine Server :
```
$ ping 10.2.0.10
PING 10.2.0.10 (10.2.0.10) 56(84) bytes of data.
64 bytes from 10.2.0.10: icmp_seq=1 ttl=63 time=2.59 ms
64 bytes from 10.2.0.10: icmp_seq=2 ttl=63 time=1.89 ms
...
```

4. J'affiche de nouveau la table ARP de ma machine Router :
```
$ ip neigh show
10.2.0.10 dev enp0s8 lladdr 08:00:27:1e:71:a1 REACHABLE
10.1.0.10 dev enp0s3 lladdr 08:00:27:1e:71:a1 REACHABLE
10.2.0.1 dev enp0s8 lladdr 0a:00:27:00:00:02 DELAY
```
##### C. Manip 3
1. Je vide de nouveau les tables ARP de toutes mes machines : `sudo ip neigh flush all`
2. J'affiche la table ARP de mon pc : `$ ip neigh show`
Maintenant je l'efface : `$ sudo ip neigh flush all`
Quand je l'affiche de nouveau :
```
ip neigh show
192.168.1.55 dev wlp58s0 lladdr e0:51:63:c1:4f:16 REACHABLE
```
Après avoir attendu :
```
192.168.1.55 dev wlp58s0 lladdr e0:51:63:c1:4f:16 REACHABLE
192.168.1.254 dev wlp58s0 lladdr 6c:38:a1:24:6a:94 REACHABLE
```

##### D. Manip 4
1. 
