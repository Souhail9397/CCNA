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

➡️ le masque de sous réseau est /24, c'est à dire que les **24 premiers bits** sont conscarés à la **partie réseau**. Concrètement, pour déterminer l'adresse réseau d'une adresse IP à l'aide du masque de sous réseau, on fait un **ET Logique**. Le principe est d'écrire l'adresse IP en format binaire. Prenons une adresse IP qui appartient au réseau 192.168.1.0/24 : 192.168.1.50/24, ce qui fait, en binaire, 11000000.10101000.00000001.00110010.  
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

➡️ adresse 172.16.0.0/16
  
- 16 derniers bits pour la partie hôte, donc 00000000.00000000  
  
- Première adresse : **00000000.00000001** = 172.16.**0.1**/16  
  
- Dernière adresse : **11111111.11111110** : 172.16.**255.254**/16    
  
➡️ adresse 10.0.0.0/8  
  
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

➡️ on utilise la commande `en` (raccourci pour `enable`) pour entrer en Privileged Exec Mode  

➡️ pour voir le statut de chaque interface du routeur, on utilise la commande `show ip interface brief`  

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
  

  
 
 
  

