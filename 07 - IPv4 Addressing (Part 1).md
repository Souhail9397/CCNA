# :one: Introduction  
  
  
Dans ce cours nous allons voir comment le trafic est envoyé entre différents LANs. Le cours précédent était centré sur la couche 2 (couche liaison de données) du modèle OSI, et ce cours sera centré sur la couche 3 (couche réseau).  

# :two: Modèle OSI - Couche réseau  

➡️ Fournit une connectivité entre les hôtes dans différents réseaux (externes au LAN)  
  
➡️ Fournit un adressage logique (**adresses IP**)  
  
➡️ Assure le chemin entre la source et la destination  
  
➡️ Les routeurs opèrent sur la couche 3  
  
# :three: Introduction au routage  
  
## 🅰️ Broadcast dans un LAN  
  
<img width="1329" height="273" alt="image" src="https://github.com/user-attachments/assets/1e5790e3-6540-4ffa-9f87-bfc79d54eb2b" />  
  
Sur ce schéma, on voit un LAN : 192.168.1.0/24. Si PC1 envoie une trame en broadcast, elle sera floodée sur toutes les interfaces des switchs, sauf sur l'interface d'où provient la frame. Donc Switch1 envoie vers son interface G0/1 et G0/2. Switch2 reçoit la frame sur son interface G0/2, et l'envoie ensuite sur ses interfaces G0/0 et G0/1. C'est ce qui se passe au sein d'un même et unique LAN.   

## 🅱️ Broadcast dans deux LANs  

<img width="1328" height="330" alt="image" src="https://github.com/user-attachments/assets/8469ed4d-c43f-4385-a82b-2a452ec0378a" />  

Sur ce schéma, on voit deux LANs : 192.168.1.0/24 et 192.168.2.0/24. Il s'agit de deux réseaux différents. Dans ce cas, si PC1 envoie une trame en broadcast, elle sera floodée sur toutes les interfaces de Switch1, mais une fois la frame arrivée sur Router1, elle n'ira pas plus loin. Un broadcast ne sort jamais du LAN d'où il est émit.   
  
ℹ️ Une adresse IP est composée de :  
- **192.168.1** : partie réseau  
- **0** : partie hôte  
- **/24** : masque de sous-réseau (détermine quelle partie de l'adresse IP correpsond à la partie réseau, et quelle partie de l'adresse IP correspond à la partie hôte - *plus de détails à venir*)  

### 🏷️ En-tête IPv4  

L'en-tête IPv4 est la "carte d'identité" d'un paquet IP : il indique d'où vient le paquet, où il va et comment les routeurs doivent le traiter.  

<img width="1293" height="385" alt="image" src="https://github.com/user-attachments/assets/23bb3448-298e-4cf8-8c4f-051b341a013f" />  

#### Champs importants

- Version → IPv4 ou IPv6
- Total Length → taille totale du paquet
- TTL → diminue à chaque routeur, évite les boucles
- Protocol → TCP / UDP / ICMP...
- Source IP → adresse de l'expéditeur
- Destination IP → adresse du destinataire
- Header Checksum → vérifie l'intégrité de l'en-tête  
  
  

  
  
  
