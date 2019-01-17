# PROJET RESEAU - TP3
## PLUSIEURS RESEAUX : ROUTAGE STATIQUE

### I. Création et utilisation simples d'une VM CentOS

La création de la VM, ainsi que l'installation de l'OS et le premier boot a été fait en classe tous ensemble.

#### 4.	Configuration réseau d'une machine CentOS

+ Pour prouver l'accès à	internet de la VM, j'ai effectué	un : `$ ping google.com`
```
PING google.com (216.58.205.14) 56(84) bytes of data.
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=1 ttl=54 time=11.6 ms
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=2 ttl=54 time=13.7 ms
...
```
+ Grâce à la même commande ainsi qu'à l'IP de notre PC hôte : `$ ping 10.33.3.26`
On obtient :
```
PING 10.33.3.26 (10.33.3.26) 56(84) bytes of data.
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=1 ttl=54 time=9.5 ms
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=2 ttl=54 time=10.2 ms

```
+ Pour afficher ma table de routage, je rentre la commande : `$ ip route`
```
default via 192.168.1.254 dev wlp58s0 proto dhcp metric 600 
172.16.4.0/24 dev vmnet8 proto kernel scope link src 172.16.4.1 
192.168.1.0/24 dev wlp58s0 proto kernel scope link src 192.168.1.36 metric 600 
192.168.102.0/24 dev vboxnet0 proto kernel scope link src 192.168.102.1 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown 
192.168.143.0/24 dev vmnet1 proto kernel scope link src 192.168.143.1 
```

#### 5. Faire joujou avec quelques commandes 
* ping :

(hôte -> VM)
``` 
 $ ping 192.168.127.10
PING 192.168.127.10 (192.168.127.10) 58(84) bytes of data.
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=1 ttl=54 time=15.1 ms
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=2 ttl=54 time=12.8 ms
...
```
(VM -> hôte)
```
$ ping 10.33.3.26
PING 10.33.3.26 (10.33.3.26) 58(84) bytes of data.
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=1 ttl=54 time=13.1 ms
64 bytes from par21s07-in-f14.1e100.net (216.58.205.14): icmp_seq=2 ttl=54 time=16.4 ms
```
* table de routage :
```

```
ou `$ ip route` (comme sur la VM)

(hôte)
```
$ ip route
default via 10.33.3.253 dev wlp58s0 proto dhcp metric 600 
10.33.0.0/22 dev wlp58s0 proto kernel scope link src 10.33.3.26 metric 600 
172.16.4.0/24 dev vmnet8 proto kernel scope link src 172.16.4.1 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown 
###192.168.127.0/24 dev vboxnet0 proto kernel scope link src 192.168.127.1### 
192.168.143.0/24 dev vmnet1 proto kernel scope link src 192.168.143.1 
``` 
ou
```
$ route -n
Table de routage IP du noyau
Destination     Passerelle      Genmask         Indic Metric Ref    Use Iface
0.0.0.0         192.168.1.254   0.0.0.0         UG    600    0        0 wlp58s0
172.16.4.0      0.0.0.0         255.255.255.0   U     0      0        0 vmnet8
192.168.1.0     0.0.0.0         255.255.255.0   U     600    0        0 wlp58s0
192.168.102.0   0.0.0.0         255.255.255.0   U     0      0        0 vboxnet0
192.168.122.0   0.0.0.0         255.255.255.0   U     0      0        0 virbr0
192.168.143.0   0.0.0.0         255.255.255.0   U     0      0        0 vmnet1
```
(VM)
```
$ ip route
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
;; ANSWER SECTION:
ynov.com.		4544	IN	A	217.70.184.38

$ dig google.com
;; ANSWER SECTION:
google.com.		3600	IN	A	216.58.209.238
```
---
### II. Notion de ports et SSH.
#### 1. Exploration des ports locaux
```
$ ss -p
u_str ESTAB      0      0      /run/dbus/system_bus_socket 22286                 * 22285                 users:(("dbus-daemon",pid=2596,fd=19))
```
#### 2.  SSH
Pour me connecter à la VM grâce à son IP en SSH, j'entre la commande :
`$ sudo ssh root@192.168.127.10`

#### 3. Firewall
##### 1. SSH


##### 2.  netcat

---

### III. Routage statique
#### 1. Préparation des hôtes

```
$ ping `192.168.112.1
PING 192.168.112.1 (192.168.112.1) 56(84) bytes of data.
64 bytes from 192.168.112.1: icmp_seq=1 ttl=64 time=1.25 ms
64 bytes from 192.168.112.1: icmp_seq=2 ttl=64 time=0.443 ms
...
```
