## :one: Introduction  
  
Dans ce cours nous allons voir comment les données circulent entre les switchs et les hôtes qui y sont connectés (PC, serveurs...), et comment fonctionne un LAN. Un LAN (Local Area Network) est un réseau contenu au sein d'une zone géographique restreinte, généralement à l'intérieur d'un bâtiment. Les réseaux WiFi domestiques et les réseaux de petites entreprises sont des exemples courants de réseaux LAN. Un LAN se situe "derrière" un routeur. Chaque LAN est composé d'appareils (PC, serveurs, téléphones...) et d'un switch, qui est lui-même connecté à un routeur. Un LAN situé à gauche d'un routeur est un LAN différent d'un LAN situé à droite d'un routeur.   

## :two: Modèle OSI - Couche liaison de données  

Pour ce cours, on se concentre sur la couche liaison de données du modèle OSI, soit la couche 2, qui :  
  
➡️ Fournit une connexion noeud à noeud ainsi que du transfert de données (par exemple, un PC à un switch, un switch à un routeur, un routeur à un routeur...)  

➡️ Définit le format de la donnée pour la transmission via un support physique  

➡️ Détècte et corrige les erreurs de la couche physique (couche 1)  

➡️ Utilise l'adressage de couche 2, distinct de l'adressage de couche 3  

➡️ Les switchs opèrent sur la couche 2  

## :three: Modèle OSI - Les PDUs  

Quand la donnée est transmise, elle passe par un processus appelé l'**encapsulation** :  

➡️ Data = **Data**  

➡️ Data + L4 header = **Segment**  

➡️ Data + L4 header + L3 header = **Packet**  

➡️ L2 trailer + Data + L4 header + L3 header + L2 header = **Frame**    

### 📦 Contenu d'une frame  

Une frame est composée de la façon suivante : Ethernet header | Data | Ethernet trailer  

🏷️ **Ethernet header** : il contient les éléments suivants : Preamble, SFD (Start Frame Delimiter), Destination, Source, Type ou Length   
  
➡️ **Preamble** [7 bytes] :  
- 7 bytes (56 bits)  
- Série de 1 et de 0 alternés (10101010 * 7 bytes)  
- Permet aux appareils de synchroniser leurs horloges de réception  
  
➡️ **SFD (Start Frame Delimiter)** [1 byte] :   
- 1 byte (8 bits)  
- 10101011  
- Marque la fin du **preamble** et le début du reste de la **frame**  
  
➡️ **Destination & Source** [6 bytes & 6 bytes] :  
- Indiquent les appareils envoyant et recevant la **frame**  
- Correspond aux **adresses MAC** de destination et source (Media Access Control)  
- Les **adresses MAC** ont une longeur de 6 byte (48 bits) et correspondent aux adresses physiques des appareils  

➡️ **Type ou Length** [2 bytes] :  
- 2 byte (16 bits)  
- Une valeur de 1500 ou moins dans ce champ indique la **LENGTH (longeur)** du paquet encapsulé (en bytes)  
- Une valeur de 1536 ou plus indique le **TYPE** du paquet encapsulé (habituellement IPv4 ou IPv6), et la length est déterminée via d'autres méthodes  
- Exemples : IPv4 = 0x0800 (valeur hexadécimale, correspond à 2048 en décimal) - IPv6 = 0x86DD (valeur hexadécimale, correspond à 34525 en décimal)   
  
📄 **Data** : contient la donnée, le message transmis  
  
🏁 **Ethernet trailer** : il contient la FCS (Frame Check Sequence)  

➡️ **FCS (Frame Check Sequence)** [4 bytes] :  
- 4 bytes (32 bits) de longeur  
- Détècte si des données sont corrompues en exécutant un algorythme **CRC** (Cyclic Redundancy Check) sur les données reçues  

 **La longeur totale de *Ethernet header* et de *Ethernet trailer* est donc de 26 bytes**  

 ## 4️⃣ Les adresses MAC  

Les adresses MAC ont une longeur de 6 bytes (48 bits). Ce sont des adresses physiques assignée aux appareils lorsqu'ils sont conçus, elles sont uniques et propres à chaque appareil. On peut aussi les retrouver sous le nom de "Burned-In Adress" (BIA). Les 3 premiers bytes sont l'**OUI** (Organizationally Unique Indeitifier). C'est un identifiant assigné aux constructeurs d'appareils, et on retrouve le même **OUI** dans tous les appareils conçus par ce constructeur. Un constructeur différent aura un **OUI** différent sur les adresses MAC de ses appareils. Les 3 derniers bytes sont uniques à l'appareil. Les adresses MAC sont écrites en hexadécimal (12 caractères).  

Les switchs apprennent les adresses MAC des appareils lorsqu'ils recoivent des frames, et où ils se situent par rapport à leurs interfaces. Quand un switch apprend l'adresse MAC d'un appareil et l'interface sur laquelle il est branché, il l'enregistre dans sa **MAC Address Table** qu'il remplit au fur et à mesure qu'il reççoit des frames de nouveaux appareils qu'il ne connaissait pas.  

Quand un switch reçoit une frame et ne sait pas sur laquelle de ses interfaces est branché le destinataire, on appelle la frame une **Unknown Unicast Frame**. Dans ce cas, le switch va **flood** la frame, c'est-à-dire l'envoyer sur TOUTES ses interfaces, sauf sur celles sur laquelle il a reçu la frame. Les appareils dont l'adresse MAC ne correspond pas vont ignorer la frame. L'appareil dont l'adresse MAC reçoit normalement le paquet.  

➡️ Exemple (3 PCs, 1 switch) : PC1 envoie une frame avec pour destinataire l'adresse MAC de PC2 (**Unicast Frame** : une frame destinée à un seul destinataire). Le switch reçoit la frame de la part de PC1, et enregistre son adresse MAC avec l'interface associée (exemple : MAC .0001 | Interface F0/1). Le switch va flood la frame et l'envoyer aux deux PCs du LAN : PC2 et PC3. PC3 reçoit la frame, voit que l'adresse MAC de destination n'est pas la sienne, donc ignore la frame. PC2 quant à lui, reçoit la frame, voit que l'adresse MAC correspond à la sienne, et va lire le paquet qui se trouve dans la frame. Si PC2 répond à PC1, il envoie la frame au switch. C'est seulement à ce moment que le switch enregistre l'adresse MAC de PC2 dans sa table d'adresses MAC, donc une nouvelle ligne y apparait (MAC .0002 | Interface F0/2). Ensuite, le switch connaît déjà l'emplacement de PC1 donc il n'a pas à flood la frame (c'est ce qu'on appelle **Known Unicast Frame**). Il envoie directement la frame à PC1.  

Quand un switch apprend les adresses MAC de cette manière, on appelle ça des "Dynamic MAC addresses" et elles sont supprimées de la table d'adresses MAC après 5 minutes d'inactivité.   




   



   
