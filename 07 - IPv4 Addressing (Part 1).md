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

# :four: Les adresses IPv4  
  
 Une adresse IP est l'adresse d'un appareil sur un réseau. Elle permet d'identifier l'appareil et de lui envoyer des données.  
 Une adresse IP fait 32 bits de long. Prenons par exemple l'adresse 192.168.1.254 :  
 - 192 : 8 bits - 11000000  
 - 168 : 8 bits  - 10101000  
 - 1 : 8 bits - 00000001   
 - 254 : 8 bits - 11111110  

 La manière d'écrire des chiffres avec des 1 et des 0 s'appelle le binaire, et c'est un mode d'écriture difficile à lire pour nous humains, mais les machines (PC, serveurs, téléphones, imprimantes...) dialogent en binaire. C'est pour cette raison qu'on convertit les adresses IPv4 en **décimal** (BINAIRE : 11000000.10101000.00000001.11111110 - DÉCIMAL : 192.168.1.254).  

## 0️⃣ Système binaire  
  
Le système décimal est en base 10 et le système binaire est en base 2, c'est à dire qu'on n'a seulement les valeurs 0 et 1. Prenons l'adresse 192.168.1.254 :  
  
<img width="1013" height="109" alt="image" src="https://github.com/user-attachments/assets/56641c61-b421-416f-a9d1-bdf0dab68830" />  
  
Pour convertir un chiffre décimal en binaire, on se base toujours sur la ligne des puissances, donc retenir | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |. On ajoute un 1 au dessus de chaque puissance qui rentre dans le chiffre décimal qu'on veut convertir. Dans l'exemple ci-dessus, 128 + 64 = 192, donc on n'a mis un 1 que sur 128 et sur 64. Si on ajoute des 1 ailleurs, par exemple sur 16, cela aurait donné 128 + 64 + 16 = 208.    

<img width="1012" height="107" alt="image" src="https://github.com/user-attachments/assets/c8dfae68-4310-43f2-b84a-c543b07106af" />  
  
<img width="1014" height="112" alt="image" src="https://github.com/user-attachments/assets/dd9078fc-4980-49e4-ab85-4faa3f0b0ed2" />  
  
<img width="1010" height="111" alt="image" src="https://github.com/user-attachments/assets/5a85217c-d617-4e0d-907c-49fda4095652" />  
  
## 🎭 Le masque de sous-réseau  

Dans l'adresse IP 192.168.1.254/24, on a vu que le /24 correspondait au masque de sous-réseau. Il permet d'identifier la partie de l'adresse IP qui représente la partie réseau, et celle qui représente la partie hôte. /24 signifie que les 24 premiers bits de l'adresse IP correspondent à la partie réseau.  
  
Pour rappel, une adresse IP est constituée de 4 octets de 8 bits chacun, donc 32 bits au total. Voyons de plus près l'adresse 192.168.1.254 en binaire :  

1er octet (192) : 11000000  
2ème octet (168) : 10101000  
3ème octet (1) : 00000001  
4ème octet (254) : 11111110  

Si les 24 premiers bits représentent la partie réseau, alors la partie réseau correspond à 11000000.10101000.00000001 (8 bits * 3 octets = 24 bits). L'adresse réseau correspond à 192.168.1.0/24.  

Pour ce qui est de la partie hôte, c'est simple : les bits restants représentent la partie hôte. Donc 32 bits - 24 bits = 8 bits. Les 8 derniers bits, soit le dernier octet complet, correspond à la partie hôte. Dans notre cas, 192.168.1.254/24 correspond à un hôte du réseau 192.168.1.0/24. Les adresses IP disponibles pour ce réseau vont de 192.168.1.1/24 à 192.168.1.254/24. Il y a toujours deux adresses IP réservées dans un réseau : la première, ici 192.168.1.0/24, qui est l'adresse réseau, et la dernière, 192.168.1.255/24, qui est l'adresse broadcast. Il est impossible de les attribuer à des hôtes, et cela vaut pour tous les réseaux.

## 📚 Classes d'adresses IPv4  
  
Les adresses IPv4 sont séparées en 5 classes différentes. Les classes sont déterminées par le premier octet d'une adresse.  

- Classe A : Premier octet 0xxxxxx, étendue du premier octet : 0-127  
- Classe B : Premier octet 10xxxxxx, étendue du premier octet : 128-191  
- Classe C : Premier octet 110xxxxx, étendue du premier octet : 192-223  
- Classe D : Premier octet 1110xxxx, étendue du premier octet : 224-239  
- Classe E : Premier octet 1111xxxx, étendue du premier octet 240-255  
   
Les classes d'adresses sur lesquelles nous allons nous concentrer sont les A B et C. Les classes D sont reservées pour les adresses **multicast** et les classes E sont reservées pour des usages experimentaux.  

Les adresses de classe A ont un **prefix length** (taille du champ réseau) de /8, classe B /16 et classe C /24. Voici des exemples d'adresses IP par classe :  
  
- Classe A : **12**.128.254.23 **/8**  
- Classe B : **154.78**.111.32 **/16**  
- Classe C : **192.168.1**.254 **/24**  
  
## 🔄 Loopback Address
  
Une adresse loopback est une adresse IP qui permet à un appareil de communiquer avec lui-même. Elles sont comprises dans la plage d'adresses IP 127.0.0.0/8 et la plus connue est 127.0.0.1 (localhost).  

On les utilise pour tester la pile TCP/IP, vérifier que le réseau local de la machine fonctionne, ou encore que les paquets ne quittent jamais l'appareil. Donc en réalité, les adresses de classe A ont pour premier octet 0-126, 127 étant reservé pour les adresses loopback. 



  
  
  
  

  
  
  
  

  
  
  
