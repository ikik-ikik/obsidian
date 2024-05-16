
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

![attachments/Pasted image 20240509130313.png](_attachments/Pasted%20image%2020240509130313.png)

Puis cliquez sur :

![attachments/Pasted image 20240509131303.png](_attachments/Pasted%20image%2020240509131303.png)

Nommez le ensuit comme vous voulez et cliquez sur créer un groupe de sécurité

Et voila le croupe est créer il faut ensuit créer les règle qui vont nous intéresser. Pour ceci nous allons cliquez ensuit sur :

![_attachments/Pasted image 20240509132014.png](_attachments/Pasted%20image%2020240509132014.png)

nous allons créer une règle SSH :

 ![_attachments/Pasted image 20240509132138.png](_attachments/Pasted%20image%2020240509132138.png)
 
 Nous allons aussi créer un règle http et https :
 
 ![_attachments/Pasted image 20240509132306.png](_attachments/Pasted%20image%2020240509132306.png)


 ![_attachments/Pasted image 20240509132409.png](_attachments/Pasted%20image%2020240509132409.png)

Et voilà votre groupe de sécurité est créer et configurer

### Ensuit il vas nous falloir créer un paire de clé ssh pour ce connecter a la machine. 

Pour ce faire il faut cliquer sur :

![_attachments/Pasted image 20240509132813.png](_attachments/Pasted%20image%2020240509132813.png)

Puis cliquer sur :

![_attachments/Pasted image 20240511192916.png](_attachments/Pasted%20image%2020240511192916.png)

Nommer la comme vous le souhaitez et sélectionner "Clé SSH" puis cliquez sur  Créer une paire de clés : 

![_attachments/Pasted image 20240511193613.png](_attachments/Pasted%20image%2020240511193613.png)

Un fichier seras ensuit télécharger garder le bien précieusement car c'est grasse a ce fichier que nous allons pouvoir nous connecter a la machine que nous allons dessuit créer

### Créer la machine serveur Mattermost

Nous allons choisir une image Ubuntu 22.04 pour créer notre serveur. Pour ce faire rendez vous sur "Images" : 

![_attachments/Pasted image 20240511194026.png](_attachments/Pasted%20image%2020240511194026.png)

Puis naviguez jusqu'à trouver l'image qui nous intéresse soit :

![_attachments/Pasted image 20240511194257.png](_attachments/Pasted%20image%2020240511194257.png)
 Cliquez sur "Démarrer"

Puis nous allons configurer notre machine :

- Donner luis le nom que vous souhaitez et cliquer sur suivant
- Nous avons déjà choisi l'image donc nous pouvons aussi cliquer sur suivant
- Puis choisissez la taille du serveur. Pour être tranquille je vais opter pour :

![_attachments/Pasted image 20240511194822.png](_attachments/Pasted%20image%2020240511194822.png)

- Puis cliquez sur suivant
- Puis cliquez sur ext-net1 pour que la machine est une IPv4 publique et cliquez sur suivant
- Cliquez encore une fois sur suivant
- Puis maintenant nous allons lui affecter le groupe de sécurité précédemment créer. Cliquez donc sur le groupe de sécurité précédemment créer et sure suivant
- Puis nous allons aussi lui donner la clé ssh créer précédemment. Pour ce faire cliquez sur la clé précédemment créer
C'est tout pour le paramétrage vous pouvez déjà cliquez sur lancé l'instance

### La machine serveur étant créer il faut maintenant si connecter

Il vas falloir trouver l'ip de la machine. Pour ce faire aller dans l'onglet instance et cliquer sur le nom de la machine que vous venez de créer :

![_attachments/Pasted image 20240514193336.png](_attachments/Pasted%20image%2020240514193336.png)

Vous trouverez l'ip ici : 

![_attachments/Pasted image 20240514193500.png](_attachments/Pasted%20image%2020240514193500.png)

Aller ensuit dans mobax sur l'onglet session :

![_attachments/Pasted image 20240514193642.png](_attachments/Pasted%20image%2020240514193642.png)

Aller ensuit dans ssh et remplissez les information suivant : 

![_attachments/Pasted image 20240514194008.png](_attachments/Pasted%20image%2020240514194008.png)

- 1 : Mettez l'ip de la machine
- 2 : Cliquer sur **Advanced SSH settings** 
- Cliquer ensuit sur **Use private key** 
- Puis cliquez sur la petite icon de fichier ce qui ouvrira l'explorateur et sélectionnez le fichier télécharger laure de la création de la paire de clé SSH (je vous avez bien dit de garder ce fichier précieusement ;) )
- Pour finir cliquez sur ok

Sur l'écran suivant cliquez sur accepter :

![_attachments/Pasted image 20240514194650.png](_attachments/Pasted%20image%2020240514194650.png)

Quand on vous demanderas de vous connecter avec un utilisateur écrivez ubuntu :

![_attachments/Pasted image 20240514194810.png](_attachments/Pasted%20image%2020240514194810.png)

Cliquez sur enter

Et voilà vous êtes connecté au serveur mais pour finir il faut 
### changer les mots de passe

exécuter les commandes suivant dans le terminal 

```bash
sudo su
```

```bash
passwd
```
 
pour mettre un mot de passe a l'utilisateur root

saisirez 2 fois le mot de passe de votre choit (sécurité avant tout)


```bash
passwd -d ubuntu
```

pour supprimer le mot de passe de l'utilisateur Ubuntu

```bash
exit
```

pour revenir a l'utilisateur Ubuntu

```bash
passwd
```

pour mettre un mot de passe a l'utilisateur Ubuntu

saisirez 2 fois le mot de passe de votre choit (sécurité avant tout)

Et voilà la machine est prête pour l'installation du serveur Mattermost

# Installation du Serveur Mattermost
doc : https://docs.mattermost.com/install/install-ubuntu.html

### Ajouter le référentiel PPA du serveur Mattermost

Fait les commande suivant :
```bash
sudo rm /usr/share/keyrings/mattermost-archive-keyring.gpg
```

```bash
curl -sL -o- https://deb.packages.mattermost.com/pubkey.gpg |  gpg --dearmor | sudo tee /usr/share/keyrings/mattermost-archive-keyring.gpg > /dev/null
```


Dans une fenêtre de terminal, exécutez la commande de configuration du référentiel suivante pour ajouter les référentiels Mattermost Server :

```bash
curl -o- https://deb.packages.mattermost.com/repo-setup.sh | sudo bash -s mattermost
```

### Installer

Avant d'installer le serveur Mattermost, il est recommandé de mettre à jour tous vos référentiels et, si nécessaire, de mettre à jour les packages existants en exécutant la commande suivante :

```bash
sudo apt update
```

```bash
sudo apt install mattermost -y
```

Vous disposez désormais de la dernière version de Mattermost Server installée sur votre système.

Le chemin d'installation est `/opt/mattermost`. Le package aura ajouté un utilisateur et un groupe nommés `mattermost`. Le fichier d'unité systemd requis a également été créé mais ne sera pas défini comme actif.

Avant de procéder à l'installation nous allons mettre en place la bas de données PostgreSQL et la configurer 

### Mise en place la base de données PostgreSQL 
doc : https://docs.mattermost.com/install/prepare-mattermost-database.html

Nous allons déjà commencer a installer PostgreSQL

```bash
sudo apt install postgresql
```

Do you want to continue? [Y/n] Y

Puis nous allons créer et configurer la base de données

Pour configurer une base de données PostgreSQL à utiliser par le serveur Mattermost :

1. Connectez-vous au serveur qui hébergera la base de données et installez PostgreSQL. Consultez la documentation [PostgreSQL](https://www.postgresql.org/download/) pour plus de détails. Une fois l'installation terminée, le serveur PostgreSQL est en cours d'exécution et un compte utilisateur Linux appelé _postgres_ a été créé.
    
2. Accédez à PostgreSQL en exécutant :

```bash
sudo -u postgres psql
```

3. Créez la base de données Mattermost en exécutant :

```
postgres=# CREATE DATABASE mattermost;
```

4. Créez l'utilisateur Mattermost _mmuser_ en exécutant la commande suivante. Assurez-vous d'utiliser un mot de passe plus sécurisé que `Super2024!`.

```
postgres=# CREATE USER mmuser WITH PASSWORD 'Super2024!';
```

5. Accordez à l'utilisateur l'accès à la base de données Mattermost en exécutant :

```
postgres=# GRANT ALL PRIVILEGES ON DATABASE mattermost to mmuser;
```

6. Autorisez l'utilisateur à changer le propriétaire d'une base de données en utilisateur `mmuser`en exécutant :

```
\c mattermost
```

```
ALTER DATABASE mattermost OWNER TO mmuser;
```

7. Accordez l'accès aux objets contenus dans le schéma spécifié en exécutant :

```
GRANT USAGE, CREATE ON SCHEMA PUBLIC TO mmuser;
```

8. Quittez le terminal interactif PostgreSQL en exécutant :

```
\q
```

Ces connexions hôtes sont spécifiques à Ubuntu 20.04 et diffèrent selon la version du système d'exploitation que vous utilisez. Par exemple, dans Ubuntu 22.04, les `peer`types de connexion sont répertoriés à `sha-256`la place.

**Base de données locale (même serveur)**

Si le serveur Mattermost et la base de données sont sur la même machine :

1. Ouvrir `/etc/postgresql/{version}/main/pg_hba.conf`en tant que _root_ dans un éditeur de texte (remplacer {version} par le numéro de version de PostgreSQL que vous avez installé) Pour ma par :
```bash
sudo nano /etc/postgresql/14/main/pg_hba.conf
```
    
2. Recherchez les lignes suivantes :
    

> `local   all             all                        peer`
> 
> `host    all             all         ::1/128        ident`

3. Changer `peer`et `ident`pour `trust`:
    

> `local   all             all                        trust`
> 
> `host    all             all         ::1/128        trust`

![_attachments/Pasted image 20240516194936.png](_attachments/Pasted%20image%2020240516194936.png)

Et voila la base de données est configuré

maintenant il faut démarrer PostgreSQL : 

```bash
sudo systemctl restart postgresql
```
### Installation
doc : https://docs.mattermost.com/install/install-ubuntu.html#setup

Avant de démarrer le serveur Mattermost, vous devez modifier le fichier de configuration. Un exemple de fichier de configuration se trouve à l'adresse `/opt/mattermost/config/config.defaults.json`.

Renommez ce fichier de configuration avec les autorisations correctes :

```bash
sudo install -C -m 600 -o mattermost -g mattermost /opt/mattermost/config/config.defaults.json /opt/mattermost/config/config.json
```

et nous allons commencer a configurer ce qu'il faut dans le fichier de config. Pour ce faire :

```bash
sudo nano /opt/mattermost/config/config.json
```
