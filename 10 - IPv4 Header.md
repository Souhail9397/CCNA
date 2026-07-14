# :one: Introduction   
  
L'en-tête IPv4 est ajouté à un segment TCP ou UDP afin de former un paquet IPv4. Il contient toutes les informations nécessaires aux routeurs pour acheminer le paquet jusqu'à sa destination (routage).  
  
<img width="1293" height="385" alt="image" src="https://github.com/user-attachments/assets/46369c8a-3daa-44a0-8a2b-54b3cfa8726d" />

  
# :two: Encapsulation  
  
Les données traversent les couches du modèle OSI selon l'ordre suivant :  
  
Données  
    ↓  
Segment (TCP/UDP)  
    ↓  
Paquet (IPv4)  
    ↓  
Trame Ethernet  
  
Couche 4 → Segment  
Couche 3 → Packet  
Couche 2 → Frame  

# :three: Les champs de l'en-tête IPv4  
  
➡️ **Version (4 bits)**  
  
Indique la version du protocole IP utilisée.  
  
IPv4 = 4   
IPv6 = 6  
  
➡️ **IHL (Internet Header Length) (4 bits)**  
  
Indique la longueur de l'en-tête IPv4 uniquement.  
   
- exprimée par multiples de 4 octets  
- minimum = 20 octets  
- maximum = 60 octets  
  
⚠️ *Ne pas confondre avec Total Length, qui correspond à la taille du paquet complet.*
  
➡️ **DSCP (6 bits)**  
  
Utilisé pour la **QoS (Quality of Service)**. Permet de donner une priorité plus élevée aux flux sensibles à la latence (VoIP, visioconférence, streaming...).  
  
➡️ **ECN (2 bits)**  
  
Permet de signaler une congestion du réseau sans devoir supprimer les paquets.  
  
➡️ **Total Length (16 bits)**  
  
Indique la taille totale du paquet IPv4 :  
  
- en-tête IPv4  
- données encapsulées (TCP, UDP, ICMP...)  
  
Valeurs :  
  
- minimum : 20 octets  
- maximum : 65 535 octets  
  
➡️ **Identification (16 bits)**    
   
Utilisé lors de la fragmentation. Tous les fragments d'un même paquet possèdent le même numéro d'identification afin d'être réassemblés à l'arrivée.  
  
➡️ **Flags (3 bits)**  
  
Contrôle la fragmentation.  
  
Les trois bits sont :
  
- Reserved (toujours 0)  
- DF (Don't Fragment)  
- MF (More Fragments)  
- DF = 1 → interdiction de fragmenter.  
- MF = 1 → d'autres fragments suivent.  
  
➡️ **Fragment Offset (13 bits)**  
  
Indique la position d'un fragment dans le paquet d'origine.  
  
Permet au destinataire de reconstruire correctement le paquet même si les fragments arrivent dans le désordre.  
  
➡️ **TTL (Time To Live) (8 bits)**  
  
Empêche les boucles de routage. Chaque routeur décrémente le TTL de 1. Lorsque le TTL atteint 0, le paquet est supprimé.  
  
➡️ **Protocol (8 bits)**  
  
Indique le protocole de couche 4 encapsulé.  
  
Valeurs importantes à connaître :  
  
| **Valeur** | **Protocole**  |
|------------|----------------|
| 1	         | ICMP           |
| 6	         | TCP            |
| 17	       | UDP            |
| 89	       | OSPF           |
  
➡️ **Header Checksum (16 bits)**  
  
Permet de vérifier l'intégrité de l'en-tête IPv4.  
  
⚠️ **Il ne vérifie pas les données transportées.**

La vérification des données est réalisée par TCP ou UDP.  
  
➡️ **Source IP Address (32 bits)**  
  
Adresse IPv4 de l'expéditeur.  
  
➡️ **Destination IP Address (32 bits)**    
  
Adresse IPv4 du destinataire.  

➡️ **Options**  
  
Champ optionnel, très rarement utilisé. S'il est présent, la longueur de l'en-tête est supérieure à 20 octets.  

# :four: Fragmentation  
  
La fragmentation se produit lorsqu'un paquet dépasse le MTU d'un lien (généralement 1500 octets sur Ethernet).  
  
Chaque fragment possède :  

- le même champ **Identification**   
- un **Fragment Offset** différent   
- un indicateur **More Fragments (MF)**    
  
Le réassemblage est effectué par l'hôte destinataire.  
  
# :five: Analyse Wireshark   
  
On retrouve tous les champs de l'en-tête IPv4 si on analyse une capture Wireshark :  
   
Version  
IHL  
DSCP  
ECN  
Total Length  
Identification  
Flags  
Fragment Offset  
TTL  
Protocol  
Header Checksum  
Source IP  
Destination IP  

Nous allons étudier cette capture :    
   
<img width="1217" height="573" alt="image" src="https://github.com/user-attachments/assets/8367bf14-423d-4423-bc81-c935e65f5aa4" />  

➡️ On se concentre sur la partie `Internet Protocol Version 4, Src : 192.168.1.1, Dst : 192.168.1.2`  

➡️ On retrouve ces valeurs en hexadécimal en bas de la capture sur la partie `45 00 00 64 00 05 00 00 ff 01 38 40 c0 a8 01 01 c0 a8 01 02`  
  
➡️ Chaque groupe de 2 caractères hexadécimaux = 1 octet = 8 bits. Exemple : 00 64 = 00 = 1 octet (soit 8 bits) et 64 = 1 octet (soit 8 bits) donc un total de 2 octets et 16 bits  
   
➡️ On retrouve les champs suivants :  
| Octet | Valeur (hex) | Signification           |
|-------|--------------|-------------------------|
| 1     | 45           | Version + IHL           |
| 2     | 00           | DSCP + ECN              |
| 3-4   | 00 64        | Total Length            |
| 5-6   | 00 05        | Identification          |
| 7-8   | 00 00        | Flags + Fragment Offset |
| 9     | ff           | TTL                     |
| 10    | 01           | Protocol                |
| 11-12 | 38 40        | Header Checksum         |
| 13-16 | c0 a8 01 01  | Source IP               |
| 17-20 | c0 a8 01 02  | Destination IP          |
  
➡️ On interprète ces valeurs de la manière suivante :  
- **Version + IHL (45)** : 4 en hexa = 0100 en binaire -> le champ **Version** fait 4 bits de long, donc 0100 = 4 bits = 4 valeur décimal = Version 4 = **IPv4**. Pour l'**IHL**, on a une valeur de 5 en hexa = 0101 en binaire -> le champ **IHL** fait 4 bits de long, donc 0101 = 4 bits = 5 valeur décimal = IHL 5 * 4 = 20 (on multiplie toujours la valeur de l'IHL par 4, et sa valeur minimale doit être de 20)  
  
- **DSCP + ECN (00)** : on sait que **DSCP** fait 6 bits et **ECN** 2 bits, soit un total de 8 bits donc un octet. Les octets qui représentent **DSCP + ECN** est `00`. 000000 (6 premiers bits, DSCP) = DSCP 0. 00 (2 bits, ECN) = ECN 0  

- **Total Length (00 64)** : la longueur de ce champ est de 16 bits. Sur la ligne de caractères hexa, **Total Length** correspond à la valeur `00 64`.  
Pour convertir de l'hexa en décimal, on multiplie par puissances de 16. Dans ce cas, pour 0064, on fait :  
  
| Chiffre hexa     | 0        | 0        |    6     |    4     |
|------------------|----------|----------|----------|----------|
| Puissances de 16 | 16^3     | 16^2     | 16^1     | 16^0     |
| Calcul           | 0 * 16^3 | 0 * 16^2 | 6 * 16^1 | 4 * 16^0 |
| Résultat         | 0        | 0        | 96       | 4        |  

On additionne le tout : 96 + 4 = 100. La valeur du **Total Length** est de 100 octets. Autrement dit, la taille totale de ce paquet IPv4 est de 100 octets.    
  
- **Identification (00 05)** : ce champ fait 16 bits de long : 0 = 4 bits, 0 = 4 bits, 0 = 4 bits et 5 = 4 bits, soit un total de 16 bits. En binaire, 0005 en hexa = 5 en décimal. On a ici un champ **Identification** à 5.  
  
- **Flags + Fragment Offset (00 00)** : **Flags** fait 3 bits et **Fragment Offset** fait 13 bits. Tout est à 0, ce qui signifie qu'il n'y a pas de fragmentation.  
  
- **TTL (ff)** : la longueur du **TTL** est de 8 bits. f = 4 bits et f = 4 bits, ce qui fait un total de 8. En hexa, ff = 255. Donc le **TTL** est de 255. Le paquet pourra traverser 255 routeurs avant d'être supprimé.   

- **Protocol (01)** : la longueur du **Protocol** est de 8 bits. 01 en hexa = 1 en décimal. Donc la valeur du **Protocol** est de 1 (ICMP). Il s'agit donc ici d'un paquet ICMP, probablement un ping    
  
- **Source IP (c0 a8 01 01)** : la longueur de ce champ est de 32 bits. On a 4 octets, donc 4 octets * 8 bits = 32 bits. Après conversion, l'adresse IP c0.a8.01.01 = 192.168.1.1. C'est donc un PC avec cette adresse IP qui est à l'origine de l'envoi du paquet  

- **Destination IP (c0 a8 01 02)** : la longueur de ce champ est de 32 bits. On a 4 octets, donc 4 octets * 8 bits = 32 bits. Après conversion, l'adresse IP c0.a8.01.02 = 192.168.1.2. C'est donc un PC avec cette adresse IP qui est destinataire du paquet  
  
## 💡 À retenir  
  
- **Un paquet** = en-tête IPv4 + segment TCP/UDP  
- **Taille minimale de l'en-tête IPv4** : 20 octets    
- **Taille maximale** : 60 octets.  
- **Taille maximale d'un paquet IPv4** : 65 535 octets.  
- **MTU** Ethernet : 1500 octets.  
- **TTL** est décrémenté par chaque routeur.  
- **TTL** = 0 → paquet supprimé.  

**Les protocoles à retenir :**    
**ICMP** = 1  
**TCP** = 6  
**UDP** = 17  
**OSPF** = 89  

La fragmentation utilise Identification, Flags et Fragment Offset.  
Le Header Checksum protège uniquement l'en-tête IPv4, pas les données.  
