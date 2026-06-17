## :one: Introduction  

Dans ce cours nous allons aborder les notions de **ARP** avec **Requête ARP** et **Réponse ARP**, les **Tables ARP**, la fonction de **ping** avec **ICMP Echo Request** et **ICMP Echo Reply**, et les **Tables d'adresses MAC**  

Nous allons nous appuyer sur le schéma suivant :  
  
<img width="1889" height="453" alt="image" src="https://github.com/user-attachments/assets/ad474bf5-7dfd-4a4b-8450-3674567dae0b" />  

  
## :two: Switching LAN Ethernet  
  
Prenons le cas de PC1 qui envoie de la donnée à PC3 :  
- **Source IP** : 192.168.1.1  
- **Destination IP** : 192.168.1.3  
- **Source MAC** : .9D00  
- **Destination MAC** : ?  

**PC1** veut envoyer de la donnée à **PC3**, mais il doit dans un premier temps découvrir l'adresse MAC de **PC3**. Pour ce faire, il utilie un protocole appelé **ARP** (Address Resolution Protocol).  

**ARP** est utilisé pour découvrir l'adresse de **couche 2 (adresse MAC)** d'une adresse de **couche 3 connue (adresse IP**). Le processus **ARP** se fait en deux temps : une **Requête ARP** puis une **Réponse ARP**. Dans notre cas, **PC1** envoie la requête et **PC3** envoie la réponse. La **Requête ARP** est en **broadcast**, c'est-à-dire qu'elle est envoyée à tous les hôtes sur le réseau. La **Réponse ARP** est **unicast**, c'est-à-dire envoyée à seulement un hôte, celui qui envoie la requête.   
  
### 📢 Requête ARP  
  
***PC1** veut envoyer de la donnée à **PC3***  

➡️ PC1 envoie une **Requête ARP** :  
- **Source IP** : 192.168.1.1  
- **Destination IP** : 192.168.1.3  
- **Source MAC** : 0C2F.B011.9D00  
- **Destination MAC** : FFFF.FFFF.FFFF = broadcast MAC adresse, l'adresse MAC de destination utilisée lorsqu'un hôte veut envoyer une frame à tous les autres hôtes du réseau  

➡️ **Switch1** reçoit la **Requête ARP** et enregistre l'adresse MAC de **PC1** dans sa table d'adresses MAC. C'est une adresse MAC dynamique et sera supprimée après 5min d'inactivité (nouvelle ligne : MAC .9D00 | Interface G0/0)  

➡️ Étant donné que l'adresse MAC de destination est une adresse broadcast, **Switch1** envoie la **Requête ARP** à toutes ses interfaces sauf celle sur laquelle il a reçu la requête.  

➡️ **PC2** reçoit la frame et l'ignore car son adresse IP ne correspond pas à celle de destination.  

➡️ **Switch2** reçoit la frame (nouvelle ligne table adresses MAC : MAC .9D00 | Interface G0/2) et l'envoie sur ses interfaces G0/0 (**PC3**) et G0/1 (**PC4**). **C4** l'ignore car son adresse IP ne correspond pas, et **PC3** la reçoit et ne l'ignore pas car son adresse IP correspond.  
  
➡️ A ce moment, **PC3** envoie la **Réponse ARP** à **PC1**.  

### 📨 Réponse ARP  

***PC3** répond à **PC1** avec une **Réponse ARP***  

➡️ **Switch2** reçoit la **Réponse ARP** et enregistre l'adresse MAC de **PC3** dans sa table d'adresses MAC (nouvelle ligne : MAC .3900 | Interface G0/0). **Switch2** envoie la frame à **Switch1**  

➡️ Ici, la **Réponse ARP** est une **known unicast frame** car l'adresse MAC du destinataire (**PC1**) est connue dans la table d'adresse MAC de **Switch2**. De ce fait, la frame ne sera pas flood sur toutes les interfaces.  

➡️ **Switch1** reçoit la frame et consulte sa table d'adresses MAC. L'adresse MAC de destination (**PC1**) est enregistrée, alors **Switch1** envoie la frame directement à **PC1**, sans flood.  

➡️ **PC1** utilise les informations contenues dans la **Réponse ARP** pour enregistrer PC3 dans sa **Table ARP**    

### 📋 Table ARP  

Pour consulter la **Table ARP** d'un PC, on tape la commande `arp -a` dans un terminal (fonctionne sur Windows, MacOS, Linux). Voici à quoi ressemble une **Table ARP** :  

<img width="462" height="234" alt="image" src="https://github.com/user-attachments/assets/e543e248-e5b1-45d1-a5ca-44b47ad67579" />  

➡️ Adresse Internet : adresse IP  

➡️ Adresse physique : adresse MAC  

➡️ Type "statique" : entrée par défaut  

➡️ Type "dynamique" : apprise par **ARP**  

## :three: Le ping  

Le **ping** est un outil réseau utilisé pour tester la connectivité entre deux machines. Il mesure le temps de communication aller-retour (ex : entre **PC1** et **PC3**). Comme **ARP**, le **ping** utilise deux messages : **ICMP Echo Request** et **ICMP Echo Reply**.  
Par contre, contrairement à la **Requête ARP**, **ICMP Echo Request** ne sera pas broadcast sur le réseau. **ICMP Echo Request** est envoyé à un hôte spécifique dont l'adresse MAC est déjà connue par le PC initiant le **ping**.  

Pour ping, on utilise simplement la commande `ping [adresse.ip]`. Voyons le processus de **ping** en détail (sur un terminal Cisco) :  

<img width="1213" height="263" alt="image" src="https://github.com/user-attachments/assets/adf3126a-7bb1-47a6-9114-d2f84855f8fd" />  

➡️ `ping 192.168.1.3` : **PC1** envoie une **ICMP Echo Request** (**ping**) à **PC3**  

➡️ `Sending 5, 100-byte ICMP Echos to 192.168.1.3` : par défaut, un **ping** sur Cisco IOS envoie 5 **ICMP Echo Request**, et on reçoit 5 **ICMP Echo Reply**. La taille par défaut de chaque **ping** est de 100 bytes.  

➡️ `.!!!!` : **.** signifie un ping échoué et **!** signifie un ping réussi  

➡️ `Success rate is 80 percent (4/5), round-trip min/avg/max = 20/20/22 ms` : 4 pings sur 5 ont réussi, soit un pourcentage de 80%. Le temps aller retour minimum, moyen et maximum est respectivement de 20, 20, 22 millisecondes.  

🤔 **Pourquoi y'a-t-il eu un échec de ping sur les 5 *ICMP Echo Request*** ?  

💡 C'est parce que **PC1** ne connaissait pas l'adresse MAC de **PC3**. En initiant le **ping**, **PC1** a vu que l'adresse MAC de **PC3** lui était inconnu, et a donc envoyé une **Requête ARP**. Le **.** précédant les 4 **!** correspond donc à la **Requête ARP**. Une fois que **PC3** a reçu la **Requête ARP**, il a envoyé une **Réponse ARP** à **PC1**, et les prochaines **ICMP Echo Request** ont correctement abouti.   

### 🦈 Analyse d'un ping sur Wireshark  

Wireshark est un puissant outil indispensable à l'analyse des paquets qui circulent sur un réseau. Nous allons étudier une capture Wireshark du **ping** entre **PC1** et **PC3**  

<img width="1330" height="518" alt="image" src="https://github.com/user-attachments/assets/a26b52ae-17ff-41a3-8c52-8299ee4b4af2" />  

➡️ **Ligne 2** : **PC1** envoie la **Requête ARP** en demandant *Who has 192.168.1.3 (adresse IP de **PC3**)? Tell 192.168.1.1 (adresse IP de **PC1**)*  
➡️ **Ligne 3** : **PC3** répond avec une **Réponse ARP**, en disant à **PC1** que *192.168.1.3 is at 0c:2f:b0:6a:39:00*  
➡️ **Ligne 4** : **PC1** ping en envoyant une **ICMP Echo Request** à **PC3**  
➡️ **Ligne 5** : **PC3** répond à **PC1** avec une **ICMP Echo Reply**  
➡️ **Ligne 6 à 11** : 3 **ICMP Echo Request** et 3 **ICMP Echo Reply** supplémentaires, jusqu'à en totaliser 4/5  

### 🧠 Commandes à retenir sur appareils Cisco (switchs, routeurs)  

- `show arp` : affiche la table ARP  
- `show mac address-table` : affiche la table d'adresses MAC  
- `clear mac address-table dynamic` : supprime toutes les adresses MAC dynamiques  
- `clear mac address-table dynamic address [adresse:mac:à:supprimer]` : supprime une adresse MAC dynamique ciblée  
- `clear mac address-table dynamic interface [interface/à/supprimer]` : supprime toutes les adresse MAC dynamiques liée à une interface  

## :four: Lab  

### 🅰️ Consignes  

*Les deux switchs ont une table d'adresses MAC vide, et tous les PCs ont une table ARP vide*  

**1 - Si PC1 ping PC3, quels messages seront envoyées sur le réseau, et quels appareils les recevront?**    

**2 - Envoyez un ping et utilisez le mode simulation de Packet Tracer pour vérifier votre réponse**  

**3 - Utilisez la fonction ping pour générer du trafic réseau et faites apprendre aux switchs les adresses MAC de tous les PCs du réseau**

**4 - Utilisez les commandes `show` sur les switchs pour identifier l'adresse MAC de chaque PC**  

**5 - Videz les adresses MAC dynamiques de la table d'adresses MAC sur chaque switch**  

### 🅱️ Réponses  

1 -   
  

  
  
  

  

  

  
