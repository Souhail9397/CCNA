Source : https://www.youtube.com/watch?v=yM-XNq9ADlI&list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ&index=6  

# 1 Introduction  

Dans ce cours nous allons étudier la suite des protocoles Internet, aussi connus sous le nom de TCP/IP. Ce sont un ensemble de protocoles qui permettent le transfert de données sur Internet. Nous allons voir certains de ces protocoles et comment ils sont utilisés dans le processus de transfert de données, à chaque étape du transfert.  

# 2 Protocoles et standards  

Un protocole est un ensemble de règles définissant comment les données doivent être communiquées entre les machines d'un réseau. Autrement dit, c'est le langage qu'utilisent les ordinateurs pour communiquer.  
Si deux machines utilisent chacune leur propre protocole, elles ne pourront pas communiquer ensemble. Au début du réseautage, c'était un problème courant. C'est pour cela que les réseaux de nos jours utilisent des protocoles standards.  
  
Un standard est une spécification convenue qui permet par exemple à un - MacBook d'accéder à un site web hébergé par un serveur sous Linux - PC Windows d'envoyer un email pouvant être lu sur un smartphone sous Android.  

## a Histoire  

- Le réseau tel que nous le connaissons aujourd'hui a commencé à être développé dans les **années 1960**  
- Le ministère de la défense des États-Unis a créé **ARPANET** le **29 Octobre 1969**, permettant aux données d'être envoyées en petits paquets et permettant aux ordinateurs de communiquer simplement entre eux  
- **ARPANET** utilisait initialement un protocole appelé **NCP (Network Control Program)**  
- En 1974, **Vinton Cerf** et **Robert Kahn** conçoivent le premier protocole de communication par paquets entre deux ordinateurs (et toujours utilisé aujourd'hui) : le protocole **TCP/IP  (Transmission Control Protocol / Internet Protocol)**    
- **ARPANET** est entièrement passé au protocole TCP/IP le **1er Janvier 1983**  
- **TCP/IP** s'est popularisé auprès des fournisseurs de solutions car il a été publié comme étant un ensemble de standards pouvant être implémentés par les fournisseurs et pouvant être utilisé sur la plupart des réseaux  

<img width="6000" height="3375" alt="image" src="https://github.com/user-attachments/assets/b0c95413-bec1-4fd1-bcd9-8a4b31733cc5" />  

 ## b Concepteurs des standards  

 La majorité des standards sont développés par des organisations de standards indépendantes avec la participation d'ingénieurs de plusieurs entreprises. On retrouve notamment l'IEEE et l'IETF :  

<img width="702" height="150" alt="image" src="https://github.com/user-attachments/assets/74620496-785f-44ec-9922-661f8170519d" />  

- **IEEE (Institute of Electrical and Electronics Engineers)** : développe plusieurs technologies utilisées sur les réseaux locaux comme **thernet (802.3)** et **Wi-Fi (802.11)**  

- **IETF (Internet Engineering Task Force)** : communauté ouverte qui définit les protocoles utilisés sur Internet : **TCP, IP, UDP, HTTP, DNS...**. Publie les standards dans des documents appelés **RFCs (Requests For Comments)**    

# 3 Modèle en couches  

## a Le modèle TCP/IP    
  
Les réseaux passent par plusieurs étapes pour faire circuler les données entre les machines : transmission physique des signaux, envois locaux sur LAN, routage entre réseaux...  Un modèle nous permet de regrouper les tâches connexes en couches. Chaque couche a un rôle spécifique dans la transmission de données et chaque couche utilise le service de la couche précédente et fournit des services à la couche suivante. Les protocoles que l'on trouve sur une couche ne peuvent pas être retrouvés sur une couche différente. Une couche peut contenir plusieurs protocoles comme **IP, TPC, HTTP...**  

<img width="2560" height="1429" alt="image" src="https://github.com/user-attachments/assets/04c82c75-e2fa-4255-b27f-791240ddfb91" />   
  
Prenons l'exemple d'un envoi de courrier par La Poste :  

**Couche Contenu** : le texte de la lettre                                                        ➡️ **Couche Application | Application Layer**   
**Couche Destinataire** : "à Bob"                                                                 ➡️ **Couche Transport | Transport Layer**  
**Couche Adresse** : adresse du destinataire                                                      ➡️ **Couche Internet | Internet Layer**  
**Couche de Livraison locale** : livraison au centre de tri le plus proche                        ➡️ **Couche de Réseau local | Local Network Layer**  
**Couche d'infrastructure** : la route qui permet à la voiture de livraison d'atteindre l'adresse ➡️ **Couche Physique | Physical Layer**

## b Rôle de chaque couche  

- **Application layer** : protocoles pour la communication entre processus d'application, créé et interprète la donnée  
- **Transport layer** : fournit une communication de bout en bout entre les processus d'application utilisant des numéros de ports  
- **Internet layer** : fournit une communication de bout en bout entre les hôtes à travers le réseau utilisant des adresses IP et des routeurs  
- **Local Network layer** : fournit une livraison de saut à saut dans un réseau local utilisant des adresses MAC et des switches  
- **Physical layer** : envoie des bits sous forme de signaux radio, électriques ou optiques sur le support physique
  

  
  
  
  

  
