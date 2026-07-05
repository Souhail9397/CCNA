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

  

