
### Installation de mobax
- Installation de mobax dans Windows
- Installation de mobax dans linux

### Création de la Vm Serveur Ubuntu sur OpenStack

### Mise en place de la base de données Postgresql
 - https://docs.mattermost.com/install/prepare-mattermost-database.html

### Mise en place de Mattermost
 - https://docs.mattermost.com/install/install-ubuntu.html

### Configuration du nom de domaine
### Mise en place du certificat SSL Let's encrypt
- https://forum.mattermost.com/t/mattermost-recipe-using-lets-encrypt-for-tls-certificates-with-mattermost-docker/7596











# Installation de mobax
### Installation de mobax dans Windows

Il vous suffit de télécharger l'exécutable suivant : https://mobaxterm.mobatek.net/download-home-edition.html

Et de laisser l'installation par défaut

### Installation de mobax dans linux 22.04

Pour ubuntu nous allons suivre la documentation suivant : https://download.mobatek.net/mobaxterm-on-linux.html 

Nous allons commencer par installer wine ce qui nous permettra d'exécuter des programme qui tourne sous Windows : https://wiki.winehq.org/Ubuntu

Ouvrir le terminal de ubuntu

mettez a jour ubuntu :
```bash
sudo apt update
```

```bash
sudo apt upgrade
```

```bash
sudo reboot
```

Installation de wine : 
Téléchargez et ajoutez la clé du référentiel :

```bash
sudo mkdir -pm755 /etc/apt/keyrings
```

```bash
sudo wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key
```

```bash
sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/jammy/winehq-jammy.sources
```

```bash
sudo apt install -y wine-stable  
```

Puis télécharger mobax sur votre machine Ubuntu : https://mobaxterm.mobatek.net/download-home-edition.html (Il faut prendre "Portable edition")

Maintenant aller dans fils puis dans téléchargement et décompresser le zip précédemment télécharger

cliquez ensuite sur les trois petit point en haut de la fenêtre et cliquez sur open in terminal

Exécuter ensuit le commande suivantes :

```bash
wine MobaXterm_version_24.1.exe
```
 Changer les numéro de version si besoin
 Puis mobax s'ouvre :)

- Si vous avez un message d'erreur indiquant que vous avez besoin du package "wine32" ou du package "wine:i386", veuillez suivre les instructions afin de l'installer.

```bash
sudo su
```

```bash
dpkg --add-architecture i386 && apt-get update && apt-get install wine32
```

```bash
exit
```

```bash
wine MobaXterm_version_24.1.exe
```


Ce qu'il y a à savoir sur cet version :

Il s'agit d'une première version de MobaXterm compatible Wine, elle est limitée et n'a pas été testée de manière approfondie, vous pouvez donc vous attendre à des résultats différents en fonction des paramètres de votre distribution. Cependant, nous l'avons jugé suffisamment utile pour le partager avec vous, même s'il nous reste encore du travail à faire pour l'améliorer. Nous espérons que vous l'apprécierez !

source : https://download.mobatek.net/mobaxterm-on-linux.html

pour exécuter mobax il vous suffît d'exécuter la commande suivant dans le dossier ou ce trouve l'exécutable

```bash
wine MobaXterm_version_24.1.exe
```

Et voila pour mobax

je vous conseille quand même de passer par Windows :)




# Création de la Vm Serveur Ubuntu sur OpenStack

Aller sur le projet OpenStack de votre choit et connectez vous avec votre compte


Puis nous allons créer un groupe sécurité pour gérer l'ouverture des port de notre serveur Mattermost

![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509130313.png]]

Puis cliquez sur :
![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509131303.png]]

Nommez le ensuit comme vous voulez et cliquez sur créer un groupe de sécurité

Et voila le croupe est créer il faut ensuit créer les règle qui vont nous intéresser. Pour ceci nous allons cliquez ensuit sur :
![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509132014.png]]
 nous allons créer une règle SSH :
 ![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509132138.png]]
 Nous allons aussi créer un règle http et https :
 ![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509132306.png]]
 ![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509132409.png]]

Et voilà votre groupe de sécurité est créer et configurer

### Ensuit il vas nous falloir créer un paire de clé ssh pour ce connecter a la machine. Pour ce faire il faut cliquer sur :

![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240509132813.png]]
Puis cliquer sur :
![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240511192916.png]]
Nommer la comme vous le souhaitez et sélectionner "Clé SSH" puis cliquez sur  Créer une paire de clés : 
![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240511193613.png]]

Un fichier seras ensuit télécharger garder le bien précieusement car c'est grasse a ce fichier que nous allons pouvoir nous connecter a la machine que nous allons dessuit créer

### Créer la machine serveur Mattermost

Nous allons choisir une image Ubuntu 22.04 pour créer notre serveur. Pour ce faire rendez vous sur "Images" : 
![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240511194026.png]]
Puis naviguez jusqu'à trouver l'image qui nous intéresse soit :
![[Mattermost_openstack_infomaniak/_attachments/Pasted image 20240511194257.png]]
 Cliquez sur "Démarrer"

Puis nous allons configurer notre machine :

- Donner luis le nom que vous souhaitez et cliquer sur suivant
- Nous avons déjà choisi l'image donc nous pouvons aussi cliquer sur suivant
- Puis choisissez la taille du serveur. Pour être tranquille je vais opter pour 
![_attachments/Pasted image 20240511194822.png](_attachments/Pasted%20image%2020240511194822.png)

- Puis cliquez sur suivant
- Puis cliquez sur ext-net1 pour que la machine est une IPv4 publique et cliquez sur suivant
- Cliquez encore une fois sur suivant
- Puis maintenant nous allons lui affecter le groupe de sécurité précédemment créer. Cliquez donc sur le groupe de sécurité précédemment créer et sure suivant
- Puis nous allons aussi lui donner la clé ssh créer précédemment. Pour ce faire cliquez sur la clé précédemment créer
C'est tout pour le paramétrage vous pouvez déjà cliquez sur lancé l'instance

### La machine serveur étant créer il faut maintenant si connecterss


