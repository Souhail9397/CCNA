# :one: Introduction  
  
Dans ce cours, nous allons voir ce que sont la **vitesse et le mode duplex** d'une interface, l'**autonégociation de la vitesse et du mode duplex**, les **statuts d'interface**, et les **compteurs et erreurs d'interface**. Enfin, on finira sur un lab pratique Cisco Packet Tracer.  
  
# :two: Interfaces des switchs  

## 📡 Routeur  

Voici à quoi ressemble un routeur. Il s'agit plus spécifiquement du modèle **Cisco ASR 1000-X Router** :  

<img width="724" height="150" alt="image" src="https://github.com/user-attachments/assets/574cc02c-227f-408c-9777-a8f82e5264fd" />  
  
➡️ Possède 8 interfaces **SFP (Small Form-factor Pluggable)** et quelques interfaces **RJ45** pour les ports console  
  
## 🔀 Switch  

Voici un switch, il s'agit du modèle **Cisco Catalyst 9200 Switch** :  
  
<img width="648" height="123" alt="image" src="https://github.com/user-attachments/assets/ff3d3c40-c713-47b1-b2aa-ef4f914149f7" />  

➡️ Possède 4 interfaces **SFP** et 48 interfaces **RJ45**  

➡️ Les switchs sont utilisés pour y connecter des hôtes (PCs, imprimantes, serveurs...)  

➡️ Peuvent s'y connecter 48 PCs, et on peut aussi y connecter le routeur grâce à l'interface **SFP**  

## 🖧 Topologie réseau pour ce cours  

Nous allons nous appuyer sur la topologie réseau suivante pour le reste de ce cours (réseau **192.168.1.0/24**) :    
 
<img width="1016" height="539" alt="image" src="https://github.com/user-attachments/assets/85c920a4-a9a8-4d62-b231-5628abd9a96b" />  

➡️ On va se focaliser sur **Switch1** et configurer ses interfaces connectées : **F0/1, F0/2, F0/3 et F0/4**  

➡️ Nous allons également rapidement configurer les interfaces non connectées  

## ⚙️ Configuration de Switch1  
  
➡️ On accède au CLI du Switch, puis on tape la commande `enable` pour passer en mode Privileged et commencer les configurations  
  
➡️ On vérifie l'état des interfaces avec la commande `show ip interface brief` ou `sh ip int br`  
  
<img width="1291" height="549" alt="image" src="https://github.com/user-attachments/assets/40726a8d-cd1e-44f5-861c-c59cac6dac85" />  
  
On remarque que les interfaces qui sont connectées à des hôtes, donc **F0/1, F0/2, F0/3 et F0/4**, ont un **Status** (couche 1) et **Protocol** (couche 2) en up/up.  
Les autres interfaces, **F0/5 à F0/12** sont quant à elles en down/down, car elles ne sont connectées à aucun hôte. Et on a vu dans le cours précédent, que le statut par défaut sur les switchs Cisco était **down**, et sur les routeurs Cisco, en **administratively down**.  
  
➡️ Une autre commande utile est la commande `show interfaces status`  
   
<img width="1287" height="499" alt="image" src="https://github.com/user-attachments/assets/417db757-7bf2-4764-9b07-a46a4e97c9d6" />  

La colonne :
- **Port** liste toutes les interfaces  
- **Name** correspond à la description des interfaces  
- **Status** montre si l'interface est connectée ou non connectée  
- **Vlan** indique à quel Vlan appartient chaque interface. Un Vlan est un plus petit LAN dans un LAN. Par défaut, le Vlan est 1. Nous verrons ce qu'est un **trunk** dans un prochain cours  
- **Duplex** indique si l'appareil est capable d'envoyer et de recevoir de la donnée en même temps (**Full Duplex**) ou non (**Half Duplex**). Par défaut, le **Duplex** est en "auto" sur les switchs Cisco, et les switchs négocient avec les hôtes pour passer le Duplex en **Full Duplex** dans la mesure du possible  
- **Speed** est en "auto" par défaut. Les interfaces **FastEthernet** sont capables d'une vitesse jusqu'à **100 Mbps/seconde (a-100)**. "auto" signifie que l'interface va négocier avec l'hôte avec qui elle sera connectée, et essayera d'établir la vitesse en 100 Mb/s dans la mesure de la capacité de l'hôte connecté  
- **Type** indique le type de port Ethernet et la vitesse que l'interface prend en charge (**10/100 = 10/100 Mb/s**, **Base** = transmission en bande de base (baseband), **TX** = câble cuivre à paires torsadées (RJ45))  
  
### ⚡ Configuration de la vitesse et du duplex de l'interface   
  
➡️ `conf t` -> `int f0/1` -> puis définir la vitesse à **100 Mbps/s** : `speed 100`  
  
➡️ Ensuite, on configure le **Duplex** et **Full** : `duplex full`  
  
➡️ Pour des raisons pratiques, il est recommandé d'ajouter une description : `description ## to R1 ##`  
  
 <img width="1261" height="615" alt="image" src="https://github.com/user-attachments/assets/c9af409f-0c31-4b3e-ba37-964a012ac52c" />  
  
➡️ Voici ce que donne la commande `sh int status` après ces configurations, sur la ligne de l'interface sur laquelle nous sommes intervenus (**Fa0/1**) :  
    
<img width="1267" height="104" alt="image" src="https://github.com/user-attachments/assets/9cccb8e5-59e2-4a42-906f-7e4ddfa7ec90" />  
  
La colonne **Name** porte la description "## to R1 ##", le **Duplex** est passé en "full" et n'est plus en "a-full" car nous l'avons manuellement forcé à être en "full", et pareil pour la vitesse qui est passée de "a-100" à "100".  
  
➡️ En pratique, il vaut mieux laisser la **Vitesse** et le **Duplex** en auto, car les deux équipements négocient la meilleure vitesse et le meilleur duplex compatibles et on évite les problèmes de duplex mismatch (un côté en full, l'autre en half), qui provoquent des collisions, des erreurs et de mauvaises performances  
  
➡️ **Bonne pratique :** laisser `speed auto` et `duplex auto` afin que les équipements négocient automatiquement la meilleure vitesse et le meilleur mode de duplex. Ne forcer ces paramètres que si cela est nécessaire et des deux côtés du lien  
  
➡️ Pour les 3 autres interfaces, **Fa0/2, Fa0/3 et Fa0/4**, on ne configure que la description, et on laisse la **Vitesse** et le **Duplex** en auto :  
  
<img width="1265" height="205" alt="image" src="https://github.com/user-attachments/assets/194218ad-f104-4668-9867-a434c533852a" />  
  
➡️ Concernant les interfaces inutilisées, par mesure de sécurité, il vaut mieux les désactiver pour éviter que n'importe quel appareil (mal intentionné ou non) qui s'y branche sans autorisation puisse être connecté au LAN  
  
➡️ Pour éviter de devoir les désactiver une par une, on peut utiliser la **range** pour configurer plusieurs interfaces en une seule fois avec la commande `interface range f0/5 - 12`, on ajoute une description avec la commande `description ## not in use ##` puis on les désactive avec `shutdown`  
  
<img width="1297" height="342" alt="image" src="https://github.com/user-attachments/assets/5e11ef3e-5bf5-4bbb-8fa6-3cd866c17a80" />  

➡️ On peut aussi choisir de configurer deux étendues différentes avec la même commande, en tapant la commande par exemple `int range f0/5 - 6, f0/9 - 12`  
  
# :three: Full Duplex et Half Duplex  
    
### 🔌 Hubs LAN  

Un **hub** est un ancien appareil réseau qui était utilisé avant l'apparition des switchs. C'est un appareil plus simple qu'un switch. Quand un **hub** reçoit une trame, il la flood sur toutes ses interfaces comme un switch le fait avec les **Unknown unicast trames**.  
Si deux PCs tentent d'envoyer une trame en même temps, par exemple PC1 vers PC2 et PC3 vers PC1, le hub va flood les trames en même temps, ce qui va résulter en une collision de trames et aucun PC ne reçevera aucune trame intacte (il s'agit ici d'un mode **Half Duplex**). C'est ce qu'on appelle un domaine de collision. Pour éviter ces collisions, les appareils Ethernet utilisent un mécanisme appelé **CSMA/CD** :  
  
- **CSMA/CD** signifie **Carrier Sense Multiple Access with Collision Detection** (Accès multiple avec écoute de porteuse et détection de collision)   
- Avant d'envoyer des trames, les appareils "écoutent" le domaine de collision jusqu'à ce qu'ils détectent que les autres appareils n'envoient plus  
- Si une collision se produit, l'appareil envoie un signal de brouillage pour informer les autres appareils qu'une collision est survenue  
- Chaque appareil va attendre un certain temps avant d'envoyer d'autres trames  
- Le processus se répète  
  
### 💥 Domaines de collision  
  
Avec les **hubs**, voici à quoi ressemblait un domaine de collision :  
  
<img width="1177" height="619" alt="image" src="https://github.com/user-attachments/assets/92f76fcc-a2fa-4f6d-b64f-af81776b1e92" />  
  
➡️ Les **hubs** sont de simples répéteurs opérant à la **couche 1**, qui répétent les signaux qu'ils recoivent  
  
➡️ Les **switchs** opèrent à la **couche 2**, utilisant les adresses MAC pour envoyer des trames à des hôtes spécifiques. Un switch n'essaye jamais d'envoyer deux trames en même temps à un même hôte  
  
➡️ Le réseau où un **hub** était utilisé, et formait un seul domaine de collision, forme trois domaines de collision lorsqu'on remplace le **hub** par un **switch** :  
  
<img width="1146" height="562" alt="image" src="https://github.com/user-attachments/assets/dfa76627-2303-44db-baf1-0c1d376e3df7" />  
  
➡️ Grâce aux fonctions avancées dont disposent les **switchs** par rapport aux **hubs**, ces appareils peuvent opérer en **Full Duplex**, ils peuvent envoyer de la donnée librement   
  
### ↔️ Full/Half Duplex  
  
- **Half Duplex** : l'appareil ne peut pas envoyer et recevoir des données en même temps. S'il reçoit une trame, il doit attendre avant d'envoyer une trame. Les appareils connectés à un **hub** sont obligés d'opérer en **Half Duplex**   
- **Full Duplex** : l'appareil peut envoyer et recevoir des données en même temps, il n'a pas besoin d'attendre. Les appareils connectés à un **switch** peuvent opérer en **Full Duplex**  

# :four: Autonégociation de la vitesse et du mode duplex  
  
- Les interfaces qui peuvent fonctionner à différentes vitesses (10/100 ou 10/100/1000) ont des paramètres par défaut de **Speed auto** et **Duplex auto**  
- Les interfaces informent leurs appareils voisins (hôtes) de leurs capacités, et ils négocient la meilleure **vitesse** et le meilleur **duplex** qu'ils peuvent tous les deux supporter  
  
**Il existe trois types d'interfaces** :  
- **Ethernet** : vitesse de 10 Mbps/s  
- **FastEthernet** : vitesse de 10/100 Mbps/s  
- **GigabitEthernet** : vitesse de 10/100/1000 Mbps/s  

Dans ce schéma qui comporte un **switch** auquel sont connectés trois **PCs** possédant une interface de chaque type, voici ce en quoi les négociations de **vitesse** et de **duplex** resulteront :  
  
<img width="881" height="355" alt="image" src="https://github.com/user-attachments/assets/7664e37f-a4a3-4e86-898b-48235b707e4a" />  
   
➡️ Si l'autonégociation est désactivée sur l'appareil connecté au switch :  
- **Vitesse** : le switch essayera de détecter la vitesse à laquelle l'autre appareil opère. S'il échoue à détecter la vitesse, il utilisera la plus lente vitesse supportée par l'appareil en fonction de son interface (si l'autre appareil a une interface GigabitEthernet (10/100/1000 Mbps/s), le switch utilisera une vitesse de **10 Mbp/s**)  
- **Duplex** : si la vitesse est de **10** ou **100 Mbp/s**, le switch utilisera le mode **Half Duplex** et si elle est de 1000 Mbp/s ou plus, il utilisera le **Full Duplex**  

<img width="1296" height="648" alt="image" src="https://github.com/user-attachments/assets/6318e164-98a6-40bd-a6c2-ecd23b0830c3" />  

Ici, la **vitesse** et le **duplex** de chaque PC a déjà été configuré manuellement. L'autonégociation s'opère donc seulement au niveau du switch. Voici ce que chaque interface du switch arrivera à autonégocier :  
- **G0/1** : vitesse 10 Mbp/s, half duplex  
- **G0/2** : vitesse 1000 Mbp/s, full duplex  
- **G0/3** : vitesse 100 Mbp/s, half duplex  

⚠️ **L'interface G0/3 est en half duplex, et le PC qui y est connecté a été manuellement configuré en full duplex. Cela résulte en une incompatibilité de mode duplex et des collisions vont se produire, et seront la cause de faibles performances Ethernet. C'est pourquoi il faut toujours utiliser l'autonégociation sur tous les appareils du réseau**     
   
## ❌ Erreurs d'interfaces  

On peut vérifier si des erreurs ont eu lieu sur une interface avec la commande `show interfaces [interface0/0]`  

Sur notre Switch 1, nous allons vérifier s'il y a eu des erreurs sur l'interface **FastEthernet 0/2** avec la commande `show interfaces f0/2`, et on se concentre sur le dernier paragraphe :  

<img width="735" height="169" alt="image" src="https://github.com/user-attachments/assets/32e68765-ab15-4edc-b1a6-59d071da1a95" />  

A retenir :  
- **Runts** : trames qui sont plus petites que la taille minimale de trame (64 octets)  
- **Giants** : trames qui sont plus larges que la taille maximale de trame (1518 octets)  
- **CRC** : trames qui ont échoué la vérification CRC  
- **Frame** : trames qui ont un format incorrect (dû à une erreur)  
- **Input errors** : total de compteurs variés, comme les 4 ci-dessus  
- **Output errors** : trames que le switch a essayé d'envoyer, mais n'a pas réussi à cause d'une erreur

  
  
  
  
  

  
  

  

  
  

  
 

  
  
  
