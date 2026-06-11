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



   
