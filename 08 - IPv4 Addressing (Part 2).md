# :one: Introduction  
  
Dans ce cours, nous allons voir plus en détail comment déterminer le nombre maximal d'hôtes dans un réseau, l'adresse du réseau à partir d'une adresse IP donnée, l'adresse broadcast, la première adresse utilisable pour les hôtes ainsi que la dernière. Ensuite, nous allons voir comment configurer des adresses IP sur des appareils Cisco, ainsi que des commandes importantes à retenir pour la configuration des appareils Cisco. Enfin, nous terminerons sur un lab pratique Cisco Packet Tracer.  
  
# :two: Nombre maximal d'hôtes dans un réseau  
  
Si on prend l'adresse IP 192.168.1.0/24, on sait que les 24 premiers bits sont réservés à la **partie réseau** et les 8 derniers à la **partie hôte**. Pour déterminer le nombre d'hôtes maximal, on procède au calcul suivant :  
`2^8 = 256 - 2 (adresses réservées : adresse réseau et adresse broadcast, impossible à attribuer à des hôtes) = 254 adresses disponibles pour des hôtes`.  

Avec un autre exemple : adresse IP 172.16.0.0/16. Le masque de sous-réseau est /16, donc les 16 premiers bits sont reservés à la **partie réseau** et les 16 derniers à la **partie hôte**.  
`2^16 = 65 536 - 2 = 65 534 adresses disponibles pour des hôtes`.  
  
Encore un autre exemple : 10.0.0.0/8. Le masque de sous réseau est /8, donc les 8 premiers bits sont réservés à la **partie réseau** et les 24 derniers à la **partie hôte**.  
`2^24 = 16 777 216 - 2 = 16 777 214 adresses disponibles pour des hôtes`.

# :three: Adresse réseau  
  
Prenons l'adresse 192.168.1.0/24 :  

➡️ Le masque de sous réseau est /24, c'est à dire que les **24 premiers bits** sont conscarés à la **partie réseau**. Concrètement, pour déterminer l'adresse réseau d'une adresse IP à l'aide du masque de sous réseau, on fait un **ET Logique**. Le principe est d'écrire l'adresse IP en format binaire. Prenons une adresse IP qui appartient au réseau 192.168.1.0/24 : 192.168.1.50/24, ce qui fait, en binaire, 11000000.10101000.00000001.00110010.  
Puis, on écrit également le masque de sous réseau en format binaire (24 premiers bits à 1) : 11111111.11111111.11111111.00000000.  
Ensuite, on superpose l'adresse IP et le masque en binaire, et les colonnes où on retrouve un 1 sur l'adresse IP et un 1 sur le masque résultent en 1. Les colonnes avec un 0 et un 1 ou un 0 et un 0 résultent en 0.  

  11000000.10101000.00000001.00110010  
  11111111.11111111.11111111.00000000   
= **11000000.10101000.00000001.00000000**  
  
Maintenant, on convertit le résultat en décimal : **192.168.1.0/24**. Avec le **ET Logique**, nous venons de déterminer l'adresse réseau de l'adresse IP 192.168.1.50/24.  

# :four: Première et dernière adresse utilisable pour des hôtes  
  
On a vu que dans tout réseau, deux adresses étaient reservées : l'adresse réseau et l'adresse broadcast.  

La première adresse utilisable pour un hôte est l'adresse qui suit l'adresse réseau. Par exemple, pour le réseau 192.168.1.0/24, la première adresse utilisable est 192.168.1.1/24. Le masque étant /24, les 24 premiers bits sont reservés au réseau, autrement dit, les 3 premiers octets (car un octet est égal à 8 bits, donc 8*3 = 24), la partie hôte se trouve sur le dernier octet.  
  
On peut aussi écrire le dernier octet qui correspond aux 8 derniers bits assignés à la partie hôte, en format binaire : 00000000 (correspond à l'adresse réseau) auquel on ajoute un 1 : 00000001 (premier hôte du réseau), ce qui donne, en format décimal, 1. En l'intégrant à l'adresse IP, ça donne 192.168.1.1/24 comme première adresse utilisable.  

Pour déterminer la dernière adresse utilisable pour les hôtes, la procédure est similaire : on prend la partie hôte en format binaire, donc 00000000, et on passe les 7 premiers bits à 1, en laissant le dernier à 0. Ce qui donne 11111110, soit 254.  

**Exemples** :   

➡️ Adresse 172.16.0.0/16
  
- 16 derniers bits pour la partie hôte, donc 00000000.00000000  
  
- Première adresse : **00000000.00000001** = 172.16.**0.1**/16  
  
- Dernière adresse : **11111111.11111110** : 172.16.**255.254**/16    
  
➡️ Adresse 10.0.0.0/8  
  
- 24 derniers bits pour la partie hôte, donc 00000000.00000000.00000000  
  
- Première adresse : **00000000.00000000.00000001** : 10.**0.0.1**/8  
  
- Dernière adresse : **11111111.11111111.11111110** : 10.**255.255.254**/8  

# :five: Configurations IP d'appareils Cisco  
  
Pour cette partie, nous allons nous baser sur le schéma suivant :  

<img width="1274" height="562" alt="image" src="https://github.com/user-attachments/assets/f25536c3-22d0-41b2-9b11-30c194362334" />  

On voit :  

➡️ **3 LANs** : 10.0.0.0/8, 192.168.0.0/24, 172.16.255.254  

➡️ **1 routeur** : R1 avec trois interfaces :  
- **Gi0/0** 10.255.255.254, reliée au LAN 1 (10.0.0.0/8)  
- **Gi0/1** 172.16.255.254, reliée au LAN 2 (172.16.0.0/16)  
- **Gi0/2** 192.168.0.254, reliée au LAN 3 (192.168.0.0/24)  

➡️ **3 switchs** : **SW1** pour le LAN 1, **SW2** pour le LAN 2, **SW3** pour le LAN 3  

➡️ **3 PCs** : **PC1** (10.0.0.1) pour le LAN 1, **PC2** (172.16.0.1) pour le LAN 2, **PC3** (192.168.0.0) pour le LAN 3  
    
💬 **Remarque** : les 3 PCs ont chacun la première adresse utilisable dans leur LAN respectif (**.1**), et les 3 interfaces de R1 ont la dernière adresse utilisable (**.254**). Par exemple, pour le LAN 1 :  
- **adresse réseau** : 10.0.0.0/8  
- **première adresse utilisable** : 10.0.0.1/8  
- **dernière adresse utilisable** : 10.255.255.254/8  
- **adresse broadcast** : 10.255.255.255/8  

Voyons comment configurer R1 en ligne de commande (CLI) :  

➡️ On utilise la commande `en` (raccourci pour `enable`) pour entrer en Privileged Exec Mode  

➡️ Pour voir le statut de chaque interface du routeur, on utilise la commande `show ip interface brief`  

<img width="1312" height="284" alt="image" src="https://github.com/user-attachments/assets/e85fbfe3-1651-477f-a0f9-380a04dc7a83" />  

➡️ analysons les informations que nous donne la commande `show ip interface brief` :  
- **Interface** : liste les interfaces de l'appareil  
- **IP-Address** : montre les adresses IP de chaque interface. Sur la capture ci-dessus, on a la valeur "unassigned" car nous n'avons pas encore configuré R1  
- **OK?** : cette colonne n'est pas très importante, mais elle indique si l'adresse IP est valide ou non. Sur des appareils modernes, il est de toute manière impossible de configurer une adresse IP invalide.    
- **Method** : indique avec quelle méthode l'adresse IP a été assignée à l'interface. Ici, nous avons "unset" car nous n'avons pas encore assigné d'adresse IP aux interfaces.  
- **Status** : état physique de l'interface (couche 1). Si l'interface est activée, qu'un câble y est connecté et que le câble est connecté à un appareil de l'autre côté, on doit avoir "up" au lieu de "administratively down". Si on a la valeur "administratively down", c'est que l'interface a été désactivée avec la commande `shutdown`. Par défaut, toutes les interfaces des **ROUTEURS CISCO** sont en "administratively down", et toutes les interfaces des **SWITCHS CISCO** ne sont pas "administratively down" par défaut (soit "up" si bien connectées, soit "down" si pas connectées). Ici, on configure un **ROUTEUR**, donc les interfaces sont "administratively down" par défaut.  
- **Protocol** : État du protocole de couche 2 (liaison de données). Cette colonne indique si la communication au niveau de la couche liaison de données fonctionne correctement sur l'interface.    
• `up` : la couche 2 fonctionne correctement. Si la colonne **Status** est également à `up`, l'interface est pleinement opérationnelle (`up/up`).  
• `down` : la couche 2 ne fonctionne pas. Cela peut être dû au fait que l'interface est désactivée (`shutdown`), qu'aucun équipement ne répond de l'autre côté, ou à un problème de configuration de la couche 2.  
En résumé :  
• `up / up` → l'interface fonctionne normalement.  
• `administratively down / down` → l'interface a été désactivée avec la commande `shutdown`.  
• `down / down` → le lien physique est indisponible (ex. : câble débranché).  
• `up / down` → le lien physique est établi (couche 1 OK), mais la couche 2 ne fonctionne pas (problème de protocole ou de configuration).  
  
## 🔌 Configuration de l'interface GigabitEthernet 0/0  
  
➡️ Taper la commande `conf t` pour entrer en Global Config Mode  
  
➡️ Pour choisir l'interface à configurer, on tape simplement la commande `interface gigabitethernet0/0`, qu'on peut aussi raccourcir en `in g0/0`  
  
➡️ Optionnel mais recommandé, on peut ajouter une description à l'interface, pour savoir à quoi elle sert précisemment. Ici, on ajoute une description pour préciser que l'interface relie le routeur à SW1 : `description ## to SW1 ##`  
   
➡️ On attribue l'adresse IP et le masque de sous-réseau (**LAN 1 10.255.255.254/8**) choisis à l'interface avec la commande `ip address 10.255.255.254 255.0.0.0`  
  
➡️ Il faut ensuite activer l'interface avec la commande `no shutdown` ou `no sh` en version raccourcie  
  
<img width="1292" height="229" alt="image" src="https://github.com/user-attachments/assets/485cca6d-39d3-42f9-892b-a3fcf98e19e7" />  
  
➡️ Une fois les configurations terminées, on vérifie le résultat avec la commande `do show ip interface brief` ou `do sh ip int br` en version raccourcie, ce qui nous montre :  
  
<img width="1298" height="248" alt="image" src="https://github.com/user-attachments/assets/c1879037-92c4-4d0a-a5bb-b3aef44e1213" />  

Vu qu'on n'a seulement configuré l'interface **GigabitEthernet0/0**, il n'y a de nouvelles valeurs que sur cette ligne. L'adresse IP de l'interface est désormais 10.255.255.254, la colonne **Method** est passée en "manual" car nous avons configuré l'adresse IP manuellement, le **Status** est désormais en "up" grâce à la commande `no shutdown`, et le **Protocol** est également "up".   
  
## 🔌 Configuration de l'interface GigabitEthernet 0/1  

➡️ On quitte la configuration de l'interface GigabitEthernet 0/0 en tapant la commande `exit` ou `ex`, puis on entre dans la configuration de l'interface GigabitEthernet 0/1 avec la commande `int g0/1`  
   
➡️ Ici, on ajoute une description pour préciser que l'interface relie le routeur à SW2 : `description ## to SW2 ##` 
  
➡️ On assigne l'adresse IP et le masque (**LAN 2 172.16.255.254 255.255.0.0**) avec la commande `ip add 172.16.255.254 255.255.0.0`  

➡️ On active l'interface avec la commande `no sh`  

➡️ Vérification de la configuration : `do sh ip int br`    
  
<img width="1272" height="223" alt="image" src="https://github.com/user-attachments/assets/a06814e9-28a3-4050-94b7-2381633e2f9a" />  


## 🔌 Configuration de l'interface GigabitEthernet 0/2  

➡️ On quitte la configuration de l'interface GigabitEthernet 0/1 en tapant la commande `exit` ou `ex`, puis on entre dans la configuration de l'interface GigabitEthernet 0/2 avec la commande `int g0/2`  

➡️ Ici, on ajoute une description pour préciser que l'interface relie le routeur à SW3 : `description ## to SW3`   
  
➡️ On assigne l'adresse IP et le masque (**LAN 3 192.168.0.254 255.255.255.0**) avec la commande `ip add 192.168.0.254 255.255.255.0`  

➡️ On active l'interface avec la commande `no sh`  

➡️ Vérification de la configuration : `do sh ip int br`    
  
<img width="1305" height="231" alt="image" src="https://github.com/user-attachments/assets/314491f2-ceb2-444f-b4e7-75a9b49559e1" />  

👍 Nos 3 interfaces sont désormais bien configurées avec les bonnes adresses IP.    

## 🛠️ Commandes supplémentaires  

➡️ `show interfaces [interface]` : montre les informations détaillées de l'interface. On y retrouve notamment l'état de l'interface (up, down, etc.), l'adresse MAC, l'adresse IP (si configurée), la bande passante (Bandwidth), la vitesse (Speed), le nombre de paquets envoyés et reçus, le nombre d'erreurs (CRC, collisions, drops...), le temps depuis le dernier changement d'état...  

➡️ `show interfaces description` : cette commande affiche un résumé de toutes les interfaces. On y retrouve le nom de l'interface, le status (couche 1), le protocol couche 2) , la description (si on en a configuré une). Cette commande est très pratique sur un routeur ou un switch qui possède beaucoup d'interfaces. En un coup d'œil, on voit quelles interfaces sont actives et à quoi elles servent.  

➡️ `do sh int desc` : montre les descriptions des interfaces   

# :six: Lab  

## 🅰️ Consignes  

**1 -** Configurer le hostname de R1  

**2 -** Utiliser une commande "show" pour voir la liste de toutes les interfaces de R1, leurs adresses IP, status etc...  

**3 -** Configurer la bonne adresse IP sur chaque interface de R1, et activer les interfaces. Configurer les bonnes descriptions sur chaque interface  

**4 -** Utiliser une commande "show" pour vérifier de nouveau les interfaces de R1  

**5 -** Consulter la running config pour confirmer les modifications de configuration, et sauvegarder la configuration  

**6 -** Configurer l'adresse IP de PC1, PC2 et PC3  

**7 -** Pinguer de PC1 vers PC2 et PC3 pour tester la connectivité  

## 🅱️ Résultats  

**1 -**  
  
- `en`  
- `conf t`  
- `hostname R1`  
  
<img width="464" height="63" alt="image" src="https://github.com/user-attachments/assets/5c553927-b42e-471c-a231-24d0c28d3465" />


**2 -**   
  
`do sh ip int br`  
  
<img width="576" height="89" alt="image" src="https://github.com/user-attachments/assets/7ef71cc6-8bea-4fa5-a3bb-92bc7552203d" />   
   
**3 -**  
  
➡️ **Interface GigabitEthernet 0/0** :  
  
- `in g0/0`  
- `ip add 15.255.255.254 255.0.0.0`  
- `no sh`  
- `description ## to SW1 ##` 
   
<img width="658" height="143" alt="image" src="https://github.com/user-attachments/assets/554516ae-7d1f-4e06-9c83-c18db69cad53" />   

➡️ **Interface GigabitEthernet 0/1** :  
    
- `exit`  
- `in g0/1`  
- `ip add 182.98.255.254 255.255.0.0`  
- `no sh`  
- `description ## to SW2 ##`  
  
<img width="641" height="156" alt="image" src="https://github.com/user-attachments/assets/ec234098-182b-47df-9fcb-f17c58e92e96" />   

➡️ **Interface GigabitEthernet 0/2** :    
  
- `exit`
- `in g0/2`
- `ip add 201.191.20.254 255.255.255.0`  
- `no sh`  
- `description ## to SW3 ##`  
  
<img width="667" height="154" alt="image" src="https://github.com/user-attachments/assets/d058c591-8f89-4b3b-b637-5f53d084a240" />  
  
**4 -**  
  
Taper la commande `do sh ip int br`  
  
<img width="631" height="93" alt="image" src="https://github.com/user-attachments/assets/1ecd8e4c-3281-452e-804f-4261f74540c3" />
  
Les adresses IP des interfaces que nous avons configurées ont bien été prises en compte, le status des interfaces est passé en "up" suite aux commandes "no sh", et le protocol est aussi passé en "up" pour toutes les interfaces, donc tout devrait fonctionner correctement.  
  
**5 -**  
  
- Pour voir la running config : `do sh running-config`  
  
<img width="427" height="284" alt="image" src="https://github.com/user-attachments/assets/62e624db-d6e9-4c4f-b3cc-bd0e6ec99905" />  
<img width="624" height="337" alt="image" src="https://github.com/user-attachments/assets/bd759b39-758f-487f-8450-f4b911ae81b4" />  
  
- Pour sauvegarder la configuration actuelle : `do write`  
  
<img width="244" height="52" alt="image" src="https://github.com/user-attachments/assets/ed84096e-3e52-41f2-baa2-5545b31c892d" />  
  
**6 -**   
  
Pour configurer les adresses IP sur les PCs, on clique sur le PC puis on se rend dans l'onglet **Config**. Ensuite, on clique sur **FastEthernet0** situé dans la colonne à gauche de la fenêtre, et on a le champ de configuration d'adresse IPv4 et IPv6. Pour cet exercice, on se concentre sur l'IPv4.  
  
➡️ **PC1** :  
  
<img width="578" height="96" alt="image" src="https://github.com/user-attachments/assets/4864221f-b7b2-4cec-8acc-471d90dfbaf6" />  

➡️ **PC2** :  
  
<img width="578" height="97" alt="image" src="https://github.com/user-attachments/assets/6643dd22-9f27-4288-b3d1-1ab97b6a78ac" />  

➡️ **PC3** :  
  
<img width="582" height="96" alt="image" src="https://github.com/user-attachments/assets/b83d35d0-85d8-45e7-82f4-f626e2b335da" />
  
**7 -**  

➡️ **Ping de PC1 vers PC2 : ✅**  

<img width="414" height="209" alt="image" src="https://github.com/user-attachments/assets/f5e75214-a9a3-4198-9fd2-890858b6b9ea" />
  
➡️ **Ping de PC1 vers PC3 : ✅**  
  
<img width="432" height="200" alt="image" src="https://github.com/user-attachments/assets/574d7e35-1c5b-47a7-b513-6e4e3b4bddb0" />  

  
  

  

  

  
  
  

  
  

  
 
 
  

