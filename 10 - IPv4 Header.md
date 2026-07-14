# :one: Introduction   
  
L'en-tête IPv4 est ajouté à un segment TCP ou UDP afin de former un paquet IPv4. Il contient toutes les informations nécessaires aux routeurs pour acheminer le paquet jusqu'à sa destination (routage).  
  
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
  
➡️ **IHL (Internet Header Length) (6 bits)**  
  
Indique la longueur de l'en-tête IPv4 uniquement.  
   
- exprimée par multiples de 4 octets  
- minimum = 20 octets  
- maximum = 60 octets  
  
⚠️ *Ne pas confondre avec Total Length, qui correspond à la taille du paquet complet.*
  
➡️ **DSCP (2 bits)**  
  
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
- **Version + IHL (45)** : 4 en hexa = 0100 en binaire -> le champ **Version** fait 4 bits de long, donc 0100 = 4 bits = 4 valeur décimal = Version 4 = **IPv4**. Pour l'**IHL**, (question : si ihl fait 6 bits de long, pourquoi ici on n'a que 4 bits ?)

À retenir pour le CCNA
Un paquet = en-tête IPv4 + segment TCP/UDP.
Taille minimale de l'en-tête IPv4 : 20 octets.
Taille maximale : 60 octets.
Taille maximale d'un paquet IPv4 : 65 535 octets.
MTU Ethernet : 1500 octets.
TTL est décrémenté par chaque routeur.
TTL = 0 → paquet supprimé.
Les protocoles à retenir :
ICMP = 1
TCP = 6
UDP = 17
OSPF = 89
La fragmentation utilise Identification, Flags et Fragment Offset.
Le Header Checksum protège uniquement l'en-tête IPv4, pas les données.
