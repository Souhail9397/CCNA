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
  
➡️ **IHL (Internet Header Length)**  
  
Indique la longueur de l'en-tête IPv4 uniquement.  
   
- exprimée par multiples de 4 octets  
- minimum = 20 octets  
- maximum = 60 octets  
  
⚠️ *Ne pas confondre avec Total Length, qui correspond à la taille du paquet complet.*
  
➡️ **DSCP**  
  
Utilisé pour la **QoS (Quality of Service)**. Permet de donner une priorité plus élevée aux flux sensibles à la latence (VoIP, visioconférence, streaming...).  
  
➡️ **ECN**  
  
Permet de signaler une congestion du réseau sans devoir supprimer les paquets.  
  
➡️ **Total Length**  
  
Indique la taille totale du paquet IPv4 :  
  
- en-tête IPv4  
- données encapsulées (TCP, UDP, ICMP...)  
  
Valeurs :  
  
- minimum : 20 octets  
- maximum : 65 535 octets  
  
➡️ **Identification**    
   
Utilisé lors de la fragmentation. Tous les fragments d'un même paquet possèdent le même numéro d'identification afin d'être réassemblés à l'arrivée.  
  
➡️ **Flags**  
  
Contrôle la fragmentation.  
  
Les trois bits sont :
  
- Reserved (toujours 0)  
- DF (Don't Fragment)  
- MF (More Fragments)  
- DF = 1 → interdiction de fragmenter.  
- MF = 1 → d'autres fragments suivent.  
  
➡️ **Fragment Offset**  
  
Indique la position d'un fragment dans le paquet d'origine.  
  
Permet au destinataire de reconstruire correctement le paquet même si les fragments arrivent dans le désordre.  
  
➡️ **TTL (Time To Live)**  
  
Empêche les boucles de routage. Chaque routeur décrémente le TTL de 1. Lorsque le TTL atteint 0, le paquet est supprimé.  
  
➡️ **Protocol**  
  
Indique le protocole de couche 4 encapsulé.  
  
Valeurs importantes à connaître :  
  
| **Valeur** | **Protocole**  |
|------------|----------------|
| 1	         | ICMP           |
| 6	         | TCP            |
| 17	       | UDP            |
| 89	       | OSPF           |
  
➡️ **Header Checksum**  
  
Permet de vérifier l'intégrité de l'en-tête IPv4.  
  
⚠️ **Il ne vérifie pas les données transportées.**

La vérification des données est réalisée par TCP ou UDP.  
  
➡️ **Source IP Address**  
  
Adresse IPv4 de l'expéditeur.  
  
➡️ **Destination IP Address**    
  
Adresse IPv4 du destinataire.  

➡️ **Options**  
  
Champ optionnel, très rarement utilisé. S'il est présent, la longueur de l'en-tête est supérieure à 20 octets.  

➡️ **Fragmentation**  
  
La fragmentation se produit lorsqu'un paquet dépasse le MTU d'un lien (généralement 1500 octets sur Ethernet).  
  
Chaque fragment possède :  

- le même champ **Identification**   
- un **Fragment Offset** différent   
- un indicateur **More Fragments (MF)**    
  
Le réassemblage est effectué par l'hôte destinataire.  
  
➡️ **Analyse Wireshark**  
  
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

Il montre également des captures de paquets ICMP (ping) normaux et fragmentés pour illustrer le fonctionnement de la fragmentation.

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
