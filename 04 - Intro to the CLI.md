## :one: Introduction 
  
Dans ce cours nous allons voir ce qu'est un CLI, les modes d'utilisation d'un appareil Cisco, les principales commandes à connaître et les fichiers de configuration. Enfin, nous allons finir sur un lab Cisco Packet Tracer.  
  
## :two: Qu'est ce qu'un CLI ?  
  
UN **CLI (Command-Line Interface)** est l'interface qui va nous permettre de configurer des appareils Cisco (routeurs, switchs). C'est une interface en ligne de commande, donc non graphique. Il existe également une interface graphique qui porte le nom de **GUI (Graphical User Interface)**. Voici ci-dessous une image d'un **CLI** et d'un **GUI**, tous les deux ayant pour but de configurer le même appareil, mais généralement c'est le **CLI** qui est utilisé.  

<img width="1149" height="413" alt="image" src="https://github.com/user-attachments/assets/2c486600-e70b-426e-9859-091d65d78844" />  
 
## :three: Se connecter à un appareil Cisco  
  
Les appareils Cisco possèdent un port **console** qui permet d'y brancher son PC. Voici à quoi un port **console** ressemble :  

<img width="704" height="377" alt="image" src="https://github.com/user-attachments/assets/fbac9e82-1724-4a35-a9ea-ee949c6c8054" />  

Pour se connecter à l'appareil, on vient donc brancher un PC avec un **rollover cable** sur le port **console**. Il est probable qu'on ait besoin d'un adaptateur car les PCs de nos jours ne sont plus adaptés aux **rollover cables**. Voici à quoi ressemble un **rollover cable** ainsi que son adaptateur :  

<img width="624" height="302" alt="image" src="https://github.com/user-attachments/assets/72c24bab-4d0c-4ad0-8b32-16b6e7d0fe2e" />

Une fois connecté à l'appareil, on accède au **CLI**. Pour ce faire, on a besoin d'un logiciel installé sur notre PC : un **Terminal Emulator**. **PuTTy** est le **Terminal Emulator** le plus répandu.  

<img width="676" height="565" alt="image" src="https://github.com/user-attachments/assets/f94443a5-3847-4735-bcf3-1634dcf4c725" />  
  
Sur l'interface de **PuTTy**, séléctionner `Serial` puis cliquer sur `Open`, et le **CLI** apparaîtra.  

<img width="886" height="608" alt="image" src="https://github.com/user-attachments/assets/dcc6dbab-c5bc-4a2e-b5f2-f546c8c12d75" />  

Une question sera posée par le **CLI** : "Would you like to enter the initial configuration dialog? [yes/no]" ➡️ Répondre `no`  
   
## :four: Modes d'utilisation  
  
Lorsqu'on se connecte à un appareil Cisco (routeur, switch), on est par défaut en **User Executive Mode**, c'est à dire en mode d'exécution utilisateur. Dans ce mode, nos capacités sont très limitées, donc pour avoir un peu plus de pouvoir sur l'appareil, il nous faut changer de mode. Pour passer en **Privileged Exec Mode** (mode d'exécution privilégié), on tape la commande `enable`.  

Admettons qu'on s'est connectés à un routeur :  
  
**User Executive Mode** : le prompt affiche **Router[>]**    
**Privileged Executive Mode** : le prompt affiche **Router[#]**    

En **Privileged Executive Mode**, on peut voir toute la configuration de l'appareil, changer l'heure, sauvegarder le fichier de configuration, redémarrer l'appareil etc... Mais on ne peut pas changer sa configuration.  

Pour configurer l'appareil, on passe en **Global Configuration Mode** en tapant la commande `configure terminal` dans le mode **Privileged [#]**, donc on a au préalable tapé la commande `enable`.  

En **Global Configuration Mode**, le prompt affiche **Router[(config)#]**    
  
## :five: Sécuriser l'accès au routeur  
  
Il est important de sécuriser l'accès au routeur afin que personne ne puisse y accèder, que ce soit avec de bonnes ou de mauvaise intentions, et consulter sa configuration ou pire, la modifier. Pour cela, on va configurer un mot de passe d'accès au **Privileged Exec Mode** pour qu'un mot de passe soit demandé après la commande `enable`.  

Pour configurer un mot de passe : taper la commande `enable password [MotDePasseDeVotreChoix]`   

🔔 **Rappel : on configure un appareil dans le **Global Configuration Mode**, donc il faut que le prompt affiche Router[(config)#] pour mettre en place le mot de passe.**    
Avec cette méthode, le mot de passe est visible dans le fichier de configuration du routeur. C'est un risque important de sécurité auquel il nous faut remédier. Pour cela, nous allons encrypter le mot de passe pour qu'il n'apparaisse pas en texte brut dans le fichier de configuration.  

Taper la commande `service password-encryption` dans le **Global Configuration Mode - Router[(config)#]** 

Maintenant, dans le fichier de configuration, le mot de passe apparaît sous forme de chiffres et de lettres. Disons que le mot de passe choisi est "CCNA". Avant la commande `service password-encryption`, il apparaîssait sous la forme "CCNA" dans le fichier de configuration. Après la commande, il apparait ainsi : "08026F6028". Mais ce n'est toujours pas optimal niveau sécurité car c'est un hash facile à décrypter sur n'importe quel site de crackage de mot de passe Cisco, comme on peut le voir ici :  

<img width="242" height="150" alt="image" src="https://github.com/user-attachments/assets/8d2306e0-04cd-47d8-a026-fdde4fd7cc84" />  

On peut sécuriser encore plus le hash avec une autre commande : `enable secret [MotDePasseDeVotreChoix]`   

Ce type d'encryptage (**MD5 encryption**) est toujours crackable mais beaucoup plus difficilement par rapport à la première méthode.  
   
  
## :six: Fichiers de configuration  
  
Il existe deux fichiers de configuration sur les appareils Cisco :  
- **Running-config** : la configuration actuelle. Lorsqu'on entre des commandes dans le **CLI**, on modifie la **Running-config**. Elle est réinitiatlisée à chaque redémarrage de l'appareil.
- **Startup-config** : le fichier de configuration qui sera chargé après le redémarrage de l'appareil.

Pour voir la **Running-config**, on tape la commande `show running-config` dans le mode **Privileged Exec Mode - Router[#]**  

📌 **Important**: souvent, lorsqu'on configure un appareil, on veut que ces modifications soit permanentent. En d'autres termes, on veut que ces modifications soient prises en compte dans la **Startup-config** et pas seulement dans la **Running-config**. Il nous faut donc enregistrer nos modifications pour qu'elles soient prises en compte au prochain redémarrage de notre appareil.  
  
Pour enregistrer la configuration actuelle dans la **Startup-config**, on a le choix entre trois commandes : `write`, `write memory`, `copy running-config startup-config`.  
   
## :seven: Résumé  

⚙️ **Modes d'utilisation**  

➡️ Router[>] : User Exec Mode  
➡️ Router [#] : Privileged Exec Mode

⌨️  

  
