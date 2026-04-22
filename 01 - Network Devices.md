Source : https://www.youtube.com/watch?v=H8W9oMNSuwo&list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ  

## :one: Introduction  
  
Ce cours aborde les équipements qui constituent un réseau. On verra le rôle de chaque équipement dans une infrastructure, des définitions de termes de base ainsi que des traductions Anglais - Français, et enfin on terminera avec un lab Cisco Packet Tracer qui reprendra tout ce qui a été vu dans le cours.
  
## :two: Traductions de termes basiques  

- 🖧 Network devices : équipements réseau  
  
- 📡 Router : routeur  

- 🧷 Switch : commutateur  

- 🧱 Firewall : parefeu  

- 🌐 Network : réseau  

- 📍 Node : noeud  

- 🏠 LAN (Local Area Network) : réseau local   


## :three: Rôle dans l'infrastructure  

📍 **Noeuds** : un noeud désigne un ordinateur ou un serveur individuel faisant partie d'un réseau. Deux noeuds interconnectés forment un réseau, ils ne forment pas de réseau tant qu'il n'y a pas de connexion établie entre les deux.    

💻 **Client** : un client peut être un ordinateur portable, un téléphone, un PC fixe. Dans un réseau, un client est un appareil qui accède à un service rendu disponible par un serveur.  

🗄️ **Serveur** : un serveur est un ordinateur __puissant__ conçu pour stocker, traiter et distribuer des données, des applications et des services à d'autres appareils (clients) via un réseau. Ils peuvent traiter plusieurs requêtes simultanément, garantissant un accès fiable aux fichiers, aux applications et aux services en ligne.  

🧷 **Switch** : un switch est un appareil qui permet à deux hôtes d'un même LAN de communiquer ensemble. Il ne permet pas à un hôte d'un LAN1 de communiquer avec un hôte d'un LAN2. Il possède beaucoup de ports permettant d'y connecter des hôtes.   
  
📡 **Routeur** : un routeur fournit une connexion inter-LAN. C'est grâce à lui qu'un hôte LAN1 peut envoyer des données à un hôte LAN2. Par ailleurs, c'est le routeur qui permet à un réseau d'accéder à Internet, ce qu'un switch ne permet pas. Un routeur possède beaucoup moins de ports qu'un switch.   

🧱 **Firewall** : un firewall est un appareil qui supervise et contrôle le trafic réseau en se basant sur des règles configurées. Il peut être placé "à l'intérieur" du réseau ou à "l'extérieur" du réseau. Les firewalls modernes sont connus sous le terme de "Next-Generation Firewalls" lorsqu'ils incluent des capacités de filtrage plus avancées et modernes.  

🛡️ **Host-based firewall** : c'est un logiciel qui filtre le trafic entrant et sortant d'une machine hôte, comme un PC. Par exemple, `Windows Defender` est celui qu'on retrouve le plus souvent sur les machines Windows. 
  
  
## :four: Lab   

### 🅰️ Consignes  

Créer le schéma réseau représenté à la minute 16:40 de la vidéo. Utiliser les appareils suivants :  

`Cisco 2911 routers (x2)`  

`Cisco 2960 switches (x2)`  

`Cisco 5505 Firewalls (x2)`  

`PCs (x2)`  

`Servers (x2)`    

Utiliser un Laptop pour représenter "attacker" dans le schéma  

Connecter les appareils ensemble en utilisant le mode de branchement 'Automatically Choose Connection Type'


### 🅱️ Résultat

<img width="1041" height="363" alt="image" src="https://github.com/user-attachments/assets/24ceb2a3-1a8d-488f-96cf-0148ac59b67c" />
