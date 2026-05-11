Source : https://www.youtube.com/watch?v=yM-XNq9ADlI&list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ&index=6  

# :one: Introduction  

Dans ce cours nous allons étudier la suite des protocoles Internet, aussi connus sous le nom de TCP/IP. Ce sont un ensemble de protocoles qui permettent le transfert de données sur Internet. Nous allons voir certains de ces protocoles et comment ils sont utilisés dans le processus de transfert de données, à chaque étape du transfert.  

# :two: Protocoles et standards  

Un protocole est un ensemble de règles définissant comment les données doivent être communiquées entre les machines d'un réseau. Autrement dit, c'est le langage qu'utilisent les ordinateurs pour communiquer.  
Si deux machines utilisent chacune leur propre protocole, elles ne pourront pas communiquer ensemble. Au début du réseautage, c'était un problème courant. C'est pour cela que les réseaux de nos jours utilisent des protocoles standards.  
  
Un standard est une spécification convenue qui permet par exemple à un - MacBook d'accéder à un site web hébergé par un serveur sous Linux - PC Windows d'envoyer un email pouvant être lu sur un smartphone sous Android.  

## 🅰️ Histoire  

- Le réseau tel que nous le connaissons aujourd'hui a commencé à être développé dans les **années 1960**
- Le ministère de la défense des États-Unis a créé **ARPANET** le **29 Octobre 1969**, permettant aux données d'être envoyées en petits paquets et permettant aux ordinateurs de communiquer simplement entre eux  
- **ARPANET** utilisait initialement un protocole appelé **NCP (Network Control Program)**  
- En 1974, **Vinton Cerf** et **Robert Kahn** conçoivent le premier protocole de communication par paquets entre deux ordinateurs (et toujours utilisé aujourd'hui) : le protocole **TCP/IP  (Transmission Control Protocol / Internet Protocol)**    
- **ARPANET** est entièrement passé au protocole TCP/IP le **1er Janvier 1983**  
- **TCP/IP** s'est popularisé auprès des fournisseurs de solutions car il a été publié comme étant un ensemble de standards pouvant être implémentés par les fournisseurs et pouvant être utilisé sur la plupart des réseaux  
  
<img width="6000" height="3375" alt="image" src="https://github.com/user-attachments/assets/b0c95413-bec1-4fd1-bcd9-8a4b31733cc5" />  

 ## 🅱️ Concepteurs des standards  

 La majorité des standards sont développés par des organisations de standards indépendantes avec la participation d'ingénieurs de plusieurs entreprises. On retrouve notamment l'IEEE et l'IETF :  

- **IEEE (Institute of Electrical and Electronics Engineers)** : développe plusieurs technologies utilisées sur les réseaux locaux comme **thernet (802.3)** et **Wi-Fi (802.11)**  
- **IETF (Internet Engineering Task Force)** : communauté ouverte qui définit les protocoles utilisés sur Internet : **TCP, IP, UDP, HTTP, DNS...**. Publie les standards dans des documents appelés **RFCs (Requests For Comments)**    

# :three: Le modèle TCP/IP  
  
## 🅰️ Introduction     
  
Les réseaux passent par plusieurs étapes pour faire circuler les données entre les machines : transmission physique des signaux, envois locaux sur LAN, routage entre réseaux...  Un modèle nous permet de regrouper les tâches connexes en couches. Chaque couche a un rôle spécifique dans la transmission de données et chaque couche utilise le service de la couche précédente et fournit des services à la couche suivante. Les protocoles que l'on trouve sur une couche ne peuvent pas être retrouvés sur une couche différente. Une couche peut contenir plusieurs protocoles comme **IP, TPC, HTTP...**  
  
Prenons l'exemple d'un envoi de courrier par La Poste :  

**Couche Contenu** : le texte de la lettre                                                        ➡️ **Couche Application | Application Layer**   
**Couche Destinataire** : "à Bob"                                                                 ➡️ **Couche Transport | Transport Layer**  
**Couche Adresse** : adresse du destinataire                                                      ➡️ **Couche Internet | Internet Layer**  
**Couche de Livraison locale** : livraison au centre de tri le plus proche                        ➡️ **Couche de Réseau local | Local Network Layer**  
**Couche d'infrastructure** : la route qui permet à la voiture de livraison d'atteindre l'adresse ➡️ **Couche Physique | Physical Layer**  
  
## 🅱️ Rôle de chaque couche  
  
- **Application layer** : 
- protocoles pour la communication entre processus d'application, créé et interprète la donnée  
- définit comment l'application traite le format, envoie, et interprète la donnée  
- les protocoles sur cette couche définissent le format des messages et les règles pour des tâches spécifiques comme :  
-- naviguer sur des pages web **(HTTP/HTTPS)**   
-- transférer des fichiers **(FTP,TFTP)**
-- envoyer/recevoir des mails **(SMTP, POP3, IMAP)**  
- les routeurs et switchs ne se soucient pas des détails de cette couche, ils transportent seulement les messages dans le réseau

- **Transport layer** :  
- fournit une communication de bout en bout entre les processus d'application utilisant des numéros de ports  
- aussi appelée **process-to-process** ou **service-to-service**  
- utilise les numéros de port pour identifier le processus sur chaque hôte  
  -- quand un client web sur PC1 envoie une requête au serveur web hébergé sur SRV1, il adresse le message au port 80 de SRV1  
- les protocoles de cette couche incluent **UDP (User Datagram Protocol)** -simple, efficace- et **TCP (Transmission Control Protocol)** -fonctionnalités plus robustes au-delà de l'adressage de messages de baseè

- **Internet layer** :  
- fournit une communication de bout en bout entre les hôtes à travers le réseau utilisant des adresses IP et des routeurs  
- utilise les adresses IP pour identifier les hôtes dans le réseau  
  -- quand PC1 envoie un message à SRV1, il envoie le message à l'adresse IP de SRV1  
- ce sont principalement les **routeurs** qui opèrent sur cette couche, utilisant l'adresse IP de destination pour envoyer le message à sa destination finale  
- les protocoles de cette couche incluent **IP (IPv4, IPv6)** et **ICMP (Internet Control Message Protocol)**

- **Local Network layer** :  
- fait circuler les données en saut à saut dans un réseau local utilisant des adresses MAC et des switches  
- un saut est une étape sur le chemin entre deux appareils (ex : routeur -> hôte)  
- les switchs ne font pas partie du chemin entre deux appareils, ils ne servent qu'à connecter des appareils dans un même réseau local  
- utilise les adresses **MAC (Media Access Control)** pour identifier les interfaces  
  -- PC1 envoie le message à l'adresse MAC de l'interface G1 de Routeur1  
  -- Routeur1 envoie le message à l'adresse MAC de l'interface G2 de Routeur2  
  -- Routeur2 envoie le message à l'adresse MAC de l'interface de SRV1  
- les protocoles de cette couche incluent Ethernet **(IEEE 802.3)** et **Wi-Fi (IEEE 802.11)**  
  
- **Physical layer** :  
- envoie des bits sous forme de signaux radio, électriques ou optiques sur le support physique  
- définit des éléments tels que les câbles, les connecteurs, les niveaux de signal et les vitesses de liaison  

# 4️⃣ Encapsulation et désencapsulation   

L'encapsulation consiste à envelopper les données couche par couche avant de les envoyer. La désencapsulation consiste à les déballer à destination afin que le destinataire puisse les utiliser.  
 
## 📦 L'encapsulation  

*Pour le processus que nous allons détailler ci-dessous, nous prenons l'exemple de PC1 qui requête SRV1*  
  
➡️ La couche Application prépare la donnée à être envoyée sur le réseau  
➡️ A mesure que le message parcourt les couches, chaque couche encapsule la donnée avec un en-tête incluant les informations nécessaire pour cette couche (adresses IP source et destination, numéro de ports, adresses MAC...)  
➡️ La couche physique transmet les bits sous forme de signaux via le support physique  

## 🔓 La désencapsulation  

➡️ Le destinataire recoit le message sous forme de flux de bits à la couche 1  
➡️ L'appareil examine l'information dans l'en-tête de la couche 2 puis les retire (désencapsulation)  
➡️ Le processus de désencapsulation continue vers les piles du dessus : la couche 3 retire l'en-tête de la couche 3, puis la couche 4 retire l'en-tête de la couche4, et enfin la donnée est transmise à la couche Application  
➡️ L'application traite la donnée et si besoin génère une réponse qui est renvoyée à la pile de traitement et est transmise à PC1  

<img width="730" height="419" alt="image" src="https://github.com/user-attachments/assets/e588b07a-3a50-4c6d-8da9-4750834e49a4" />   

## 📄 Unités de données de protocoles  

➡️ A chaque étape du processus d'encapsulation et de désencapsulation, un nom est donné au message  
➡️ La combinaison de la donnée et de l'en-tête de la couche 4 est appelé un **segment (TCP)** ou **datagram (UDP)**  
➡️ **TCP** créé des **segments**, **UDP** crée des **datagrams**  
➡️ La combinaison d'un **segment/datagram** et d'un en-tête de couche 3 est appelé un **paquet**  
➡️ La combinaison d'un **packet** et d'un en-tête de couche 2 est appelé une **frame**  
➡️ On peut utiliser des noms alternatifs pour identifier un message à chaque étape : **Protocol Data Unit (PDU)**  
➡️ un segment ou un datagram est un **Layer 4 PDU (L4PDU)**  
➡️ un paquet est un **Layer 3 PDU (L3PDU)**  
➡️ une frame est un **Layer 2 PDU (L2PDU)**  
➡️ le contenu de chaque **PDU** (tout ce qui est encapsulé par l'en-tête de cette couche) est appelé le **payload**  
➡️ le payload d'un segment ou d'un datagram est l'**application data**  
➡️ le payload d'un paquet est un **segment** ou un **datagram**  
➡️ le payload d'une frame est un **paquet**  

## 🔗 Interactions entre couches adjacentes  

Chaque couche fournit un service à la couche du dessus, and est utilisé par la couche du dessous.  

➡️ **Couche 4** fournit un service à **Couche 5** en transmettant la donnée à l'application appropriée en utilisant le numéro de port  
➡️ **Couche 3** fournit un service à **Couche 4** en transmettant des segments ou datagrams au hôte de destination en utilisant l'adresse IP  
➡️ **Couche 2** fournit un service à **Couche 3** en transmettant les paquets au prochain point de saut (routeur) en utilisant les adresses MAC  
➡️ **Couche 1** fournit un service à **Couche 2** en envoyant et recevant des frames sous forme de signaux eletriques, optiques ou radio  

## 🤝 Interaction sur la même couche  

Chaque couche communique avec la même couche sur le deuxième appareil.  

➡️ La couche Application sur un hôte envoie la donnée à la couche Application à l'autre hôte  
➡️ Un segment/datagram est adressé au numéro de port de couche 4 de l'application appropriée sur l'hôte de destination  
➡️ un paquet est adressé à l'adresse IP de couche 3 de l'hôte de destination  
➡️ Une frame est adressée à l'adresse MAC de couche 2 du prochain point de saut  
➡️ Les signaux envoyés d'un port physique sont reçus par un port physique sur l'autre appareil  

# 5️⃣ Le modèle OSI  

➡️ TCP/IP a été développé dans les années 1970  
➡️ Entre 1970 et 1980, l'**ISO (International Organization for Standardization)** a inventé un système en 7 couches, appelé le modèle **OSI Open Systems Interconnection)**   
➡️ Les gouvernements ont désigné le modèle **OSI** comme pile recommandée pour les nouveaux déploiements informatiques   
➡️ Avec le temps, le modèle **OSI** est devenu compliqué et n'a jamais été autant utilisé que **TCP/IP**  
➡️ **TCP/IP** est redevenu le modèle recommandé d'utilisation, mais le modèle **OSI** est toujours utilisé comme base d'apprentissage aujourd'hui  
➡️ Le modèle en 5 couches **(Application, Transport, Network, Data Link, Physical)** est le modèle principalement utilisé de nos jours, et le nom des couches vient du modèle **OSI**  

<img width="1258" height="771" alt="image" src="https://github.com/user-attachments/assets/9e9cf0d1-4a08-4c93-88b1-ee83f1d59376" />  
  

## 🧠 A retenir  

### Modèle TCP/IP  

➡️ **Couche Application (5)** :  
- Regroupe les couches **OSI 7,6 et 5**
- **Protocoles** : HTTP/HTTPS, DNS, SMTP, SSH
- **Rôle** : Les applications utilisent le réseau

➡️ **Couche Transport (4)** :  
- Correspond à **OSI 4**
- **Protocoles** : TCP, UDP
- **Rôle** : Fiabilité et ports

➡️ **Couche Internet (3)** :  
- Correspond à **OSI 3**
- **Protocoles** : IP, ICMP
- **Rôle** : Adressage IP et routage

➡️ **Couche Liaison de données (2)** :  
- Correspond à **OSI 2**
- **Protocoles** : Ethernet, Wi-Fi
- **Rôle** : Transmission locale avec les adresses MAC

➡️ **Couche Physique (1)** :  
- Correspond à **OSI 1**
- **Rôle** : Transmission des bits sur câble ou radio

## 💡 Phrase mémo  

*Application parle, Transport organise, Internet route, Liaison livre localement, Physique transporte les bits.*

  




  
  

  
  
  
  

  
