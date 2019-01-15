# PROJET RESEAU - TP3
## PLUSIEURS RESEAUX : ROUTAGE STATIQUE


### 1. Création et utilisation simples d'une VM CentOS

La création de la VM, ainsi que	l'installation de l'OS et le premier boot a été	fait en classe tous ensemble.

#### 4.	Configuration réseau d'une machine CentOS

+ Pour prouver l'accès à	internet de la VM, j'ai effectué	un : `$ traceroute google.com`
```
à remplir
```
+ Grâce à la même commande ainsi qu'à l'IP de notre PC hôte : `$ traceroute 10.33.3.26`
On obtient :
```
à remplir
```
+ Pour afficher ma table de routage, je rentre la commande : `$ ip route`
```
à remplir
```

#### 5. Faire joujou avec quelques commandes 

* ping :

(hôte -> VM)
``` 
 $ ping 192.168.127.10
 $
 $
```
(VM -> hôte)
```
$ ping 10.33.3.26
$
$
```
* table de routage :
`$ ip route`

(hôte)
```
default via 10.33.3.253 dev wlp58s0 proto dhcp metric 600 
10.33.0.0/22 dev wlp58s0 proto kernel scope link src 10.33.3.26 metric 600 
172.16.4.0/24 dev vmnet8 proto kernel scope link src 172.16.4.1 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown 
###192.168.127.0/24 dev vboxnet0 proto kernel scope link src 192.168.127.1### 
192.168.143.0/24 dev vmnet1 proto kernel scope link src 192.168.143.1 
``` 
(VM)
```
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
###192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101###
```

* curl :
Ici le curl télécharge un iso Ubuntu
```
$ curl -O https://ubuntu-fr.org/telechargement\?action\=dl
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                Dload  Upload   Total   Spent    Left  Speed
100   300    0   300    0     0   1234      0 --:--:-- --:--:-- --:--:--  1234
```
* dig :
```
$ dig ynov.com


$ dig google.com


```
