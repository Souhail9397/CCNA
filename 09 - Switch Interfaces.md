# :one: Introduction  
  
Dans ce cours, nous allons voir ce que sont la **vitesse et le mode duplex** d'une interface, l'**autonégociation de la vitesse et du mode duplex**, les **statuts d'interface**, et les **compteurs et erreurs d'interface**. Enfin, on finira sur un lab pratique Cisco Packet Tracer.  
  
# :two: Interfaces des switchs  

## 📡 Routeur  

Voici à quoi ressemble un routeur. Il s'agit plus spécifiquement du modèle **Cisco ASR 1000-X Router** :  

<img width="724" height="150" alt="image" src="https://github.com/user-attachments/assets/574cc02c-227f-408c-9777-a8f82e5264fd" />  
  
➡️ Possède 8 interfaces **SFP (Small Form-factor Pluggable)** et quelques interfaces **RJ45** pour les ports console  
  
## 🔀 Switch  

Voici un switch, il s'agit du modèle **Cisco Catalyst 9200 Switch** :  
  
<img width="648" height="123" alt="image" src="https://github.com/user-attachments/assets/ff3d3c40-c713-47b1-b2aa-ef4f914149f7" />  

➡️ Possède 4 interfaces **SFP** et 48 interfaces **RJ45**  

➡️ Les switchs sont utilisés pour y connecter des hôtes (PCs, imprimantes, serveurs...)  

➡️ Peuvent s'y connecter 48 PCs, et on peut aussi y connecter le routeur grâce à l'interface **SFP**  

## 🖧 Topologie réseau pour ce cours  

Nous allons nous appuyer sur la topologie réseau suivante pour le reste de ce cours (réseau **192.168.1.0/24**) :    
 
<img width="1016" height="539" alt="image" src="https://github.com/user-attachments/assets/85c920a4-a9a8-4d62-b231-5628abd9a96b" />  

➡️ On va se focaliser sur **Switch1** et configurer ses interfaces connectées : **F0/1, F0/2, F0/3 et F0/4**  

➡️ Nous allons également rapidement configurer les interfaces non connectées  

## ⚙️Configuration de Switch1  

➡️ On accède au CLI du Switch, puis on tape la commande `enable` pour passer en mode Privileged et commencer les configurations  

➡️ On vérifie l'état des interfaces avec la commande `show ip interface brief` ou `sh ip int br`  

<img width="1291" height="549" alt="image" src="https://github.com/user-attachments/assets/40726a8d-cd1e-44f5-861c-c59cac6dac85" />  

On remarque que les interfaces qui sont connectées à des hôtes, donc **F0/1, F0/2, F0/3 et F0/4**, ont un **Status** (couche 1) et **Protocol** (couche 2) en up/up.  
Les autres interfaces, **F0/5 à F0/12** sont quant à elles en down/down, car elles ne sont connectées à aucun hôte. Et on a vu dans le cours précédent, que le statut par défaut sur les switchs Cisco était **down**, et sur les routeurs Cisco, en **administratively down**.  

➡️ Une autre commande utile est la commande `show interfaces status`  
   
<img width="1287" height="499" alt="image" src="https://github.com/user-attachments/assets/417db757-7bf2-4764-9b07-a46a4e97c9d6" />  

La colonne :
- **Port** liste toutes les interfaces  
- **Name** correspond à la description des interfaces  
- **Status** montre si l'interface est connectée ou non connectée  
- **Vlan** indique à quel Vlan appartient chaque interface. Un Vlan est un plus petit LAN dans un LAN. Par défaut, le Vlan est 1. Nous verrons ce qu'est un **trunk** dans un prochain cours  
- **Duplex** indique si l'appareil est capable d'envoyer et de recevoir de la donnée en même temps (**Full Duplex**) ou non (**Half Duplex**). Par défaut, le **Duplex** est en "auto" sur les switchs Cisco, et les switchs négocient avec les hôtes pour passer le Duplex en **Full Duplex** dans la mesure du possible  
- **Speed** est en "auto" par défaut. Les interfaces **FastEthernet** sont capables d'une vitesse jusqu'à **100 Mbits/seconde (a-100)**. "auto" signifie que l'interface va négocier avec l'hôte avec qui elle sera connectée, et essayera d'établir la vitesse en 100 Mb/s dans la mesure de la capacité de l'hôte connecté  
- **Type** indique le type de port Ethernet et la vitesse que l'interface prend en charge (**10/100 = 10/100 Mb/s**, **Base** = transmission en bande de base (baseband), **TX** = câble cuivre à paires torsadées (RJ45))

  
  
  
