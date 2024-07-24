
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

Veuillez télécharger l'exécutable à partir du lien suivant : https://mobaxterm.mobatek.net/download-home-edition.html

Ensuite, laissez les paramètres d'installation par défaut.

### Installation de mobax dans linux 22.04

Pour Ubuntu, nous allons suivre la documentation suivante : https://download.mobatek.net/mobaxterm-on-linux.html 

Nous allons commencer par installer Wine, ce qui nous permettra d'exécuter des programmes conçus pour Windows. https://wiki.winehq.org/Ubuntu

Ouvrez le terminal d'Ubuntu.

Mettez à jour Ubuntu en exécutant la commande suivante dans le terminal :
```bash
sudo apt update
```

```bash
sudo apt upgrade
```

```bash
sudo reboot
```

  
Pour installer Wine, suivez les étapes suivantes :

1. Téléchargez et ajoutez la clé du référentiel en exécutant la commande suivante dans le terminal :

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

  
Téléchargez Mobax sur votre machine Ubuntu en exécutant la commande suivante dans le terminal : https://mobaxterm.mobatek.net/download-home-edition.html (Il faut prendre "Portable edition")

Accédez au dossier 'Téléchargements', puis extrayez le fichier zip précédemment téléchargé.

  
Ensuite, cliquez sur les trois petits points situés en haut de la fenêtre, puis sélectionnez "Open in Terminal".

Ensuite, exécutez les commandes suivantes :

```bash
wine MobaXterm_version_24.1.exe
```
 Changez les numéros de version si nécessaire, puis Mobax s'ouvrira :)

- Si vous recevez un message d'erreur indiquant que vous avez besoin du package "wine32" ou du package "wine:i386", veuillez suivre les instructions pour l'installer.

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


  
Voici ce qu'il y a à savoir sur cette version :

Il s'agit d'une première version de MobaXterm compatible Wine, elle est limitée et n'a pas été testée de manière approfondie, vous pouvez donc vous attendre à des résultats différents en fonction des paramètres de votre distribution. Cependant, nous l'avons jugé suffisamment utile pour le partager avec vous, même s'il nous reste encore du travail à faire pour l'améliorer. Nous espérons que vous l'apprécierez !

Source : https://download.mobatek.net/mobaxterm-on-linux.html

Pour exécuter Mobax, il vous suffit d'exécuter la commande suivante dans le dossier où se trouve l'exécutable :

```bash
wine MobaXterm_version_24.1.exe
```

Parfait, Mobax est maintenant configuré sur votre système !

Je vous conseille quand même de passer par Windows :)




# Création de la Vm Serveur Ubuntu sur OpenStack

Allez sur le projet OpenStack de votre choix, puis connectez-vous avec votre compte.


Ensuite, nous allons créer un groupe de sécurité pour gérer l'ouverture des ports de notre serveur Mattermost.

![attachments/Pasted image 20240509130313.png](_attachments/Pasted%20image%2020240509130313.png)

Puis cliquez sur :

![attachments/Pasted image 20240509131303.png](_attachments/Pasted%20image%2020240509131303.png)

Nommez-le ensuite comme vous le souhaitez, puis cliquez sur "Créer un groupe de sécurité".

Et voilà, le groupe est créé. Ensuite, nous devons créer les règles qui nous intéressent. Pour cela, cliquez ensuite sur :

![_attachments/Pasted image 20240509132014.png](_attachments/Pasted%20image%2020240509132014.png)

Nous allons créer une règle SSH :

 ![_attachments/Pasted image 20240509132138.png](_attachments/Pasted%20image%2020240509132138.png)
 
 Nous allons également créer une règle pour HTTP et HTTPS :
 
 ![_attachments/Pasted image 20240509132306.png](_attachments/Pasted%20image%2020240509132306.png)


 ![_attachments/Pasted image 20240509132409.png](_attachments/Pasted%20image%2020240509132409.png)

 Et voilà, votre groupe de sécurité est créé et configuré !

### Ensuite, il nous faudra créer une paire de clés SSH pour se connecter à la machine. 

Pour ce faire, cliquez sur :

![_attachments/Pasted image 20240509132813.png](_attachments/Pasted%20image%2020240509132813.png)

Ensuite, cliquez sur :

![_attachments/Pasted image 20240511192916.png](_attachments/Pasted%20image%2020240511192916.png)

Nommez-la comme vous le souhaitez, sélectionnez "Clé SSH", puis cliquez sur "Créer une paire de clés" :

![_attachments/Pasted image 20240511193613.png](_attachments/Pasted%20image%2020240511193613.png)

Un fichier sera ensuite téléchargé. Gardez-le bien précieusement, car c'est grâce à ce fichier que nous pourrons nous connecter à la machine que nous allons ensuite créer.

### Créer la machine serveur Mattermost

Nous allons choisir une image Ubuntu 22.04 pour créer notre serveur. Pour ce faire, rendez-vous sur "Images" :

![_attachments/Pasted image 20240511194026.png](_attachments/Pasted%20image%2020240511194026.png)

Puis naviguez jusqu'à trouver l'image qui nous intéresse, à savoir :

![_attachments/Pasted image 20240511194257.png](_attachments/Pasted%20image%2020240511194257.png)
 
 Cliquez sur "Démarrer"

Puis nous allons configurer notre machine :

- Donnez-lui le nom que vous souhaitez et cliquez sur suivant.
- Comme nous avons déjà choisi l'image, vous pouvez également cliquer sur suivant.
- Ensuite, choisissez la taille du serveur. Pour être tranquille, je vais opter pour :

![_attachments/Pasted image 20240511194822.png](_attachments/Pasted%20image%2020240511194822.png)

- Cliquez ensuite sur suivant.
- Ensuite, cliquez sur "ext-net1" pour que la machine ait une IPv4 publique, puis cliquez sur suivant.
- Cliquez encore une fois sur suivant.
- Maintenant, nous allons lui affecter le groupe de sécurité précédemment créé. Cliquez donc sur le groupe de sécurité précédemment créé, puis sur suivant.
- Enfin, nous allons aussi lui donner la clé SSH créée précédemment. Pour ce faire, cliquez sur la clé précédemment créée.
C'est tout pour le paramétrage. Vous pouvez maintenant cliquer sur "Lancer l'instance".

### Maintenant que la machine serveur est créée, il est temps de s'y connecter.

Il va falloir trouver l'IP de la machine. Pour ce faire, allez dans l'onglet "Instance" et cliquez sur le nom de la machine que vous venez de créer.

![_attachments/Pasted image 20240514193336.png](_attachments/Pasted%20image%2020240514193336.png)

Vous trouverez l'IP ici : 

![_attachments/Pasted image 20240514193500.png](_attachments/Pasted%20image%2020240514193500.png)

Ensuite, dans Mobax, rendez-vous dans l'onglet "Session" :

![_attachments/Pasted image 20240514193642.png](_attachments/Pasted%20image%2020240514193642.png)

Ensuite, dans SSH, remplissez les informations suivantes :

![_attachments/Pasted image 20240514194008.png](_attachments/Pasted%20image%2020240514194008.png)

1.  Mettez l'IP de la machine.
2. Cliquez sur **Advanced SSH settings**.
3. Ensuite, cliquez sur **Use private key**.
4. Cliquez ensuite sur l'icône de fichier, ce qui ouvrira l'explorateur. Sélectionnez le fichier téléchargé lors de la création de la paire de clés SSH (je vous avais bien dit de garder ce fichier précieusement ;) ).
5. Pour finir, cliquez sur OK.

Sur l'écran suivant, cliquez sur "Accepter".

![_attachments/Pasted image 20240514194650.png](_attachments/Pasted%20image%2020240514194650.png)

Quand on vous demandera de vous connecter avec un utilisateur, écrivez "ubuntu".

![_attachments/Pasted image 20240514194810.png](_attachments/Pasted%20image%2020240514194810.png)

Cliquez sur "Enter".

Et voilà, vous êtes connecté au serveur ! Pour finir, il faut... 
### Changer les mots de passe

Exécutez les commandes suivantes dans le terminal :

```bash
sudo su
```

```bash
passwd
```
 
Pour mettre un mot de passe à l'utilisateur root :

Saisissez deux fois le mot de passe de votre choix (sécurité avant tout).


```bash
passwd -d ubuntu
```

Pour supprimer le mot de passe de l'utilisateur Ubuntu :

```bash
exit
```

Pour revenir à l'utilisateur Ubuntu 

```bash
passwd
```

Pour mettre un mot de passe à l'utilisateur Ubuntu

Saisissez deux fois le mot de passe de votre choix (sécurité avant tout).

Et voilà, la machine est prête pour l'installation du serveur Mattermost !

# Installation du Serveur Mattermost
doc : https://docs.mattermost.com/install/install-ubuntu.html

### Ajouter le référentiel PPA du serveur Mattermost

Exécutez les commandes suivantes :
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

Et voilà, la base de données est configurée.

Maintenant, il faut démarrer PostgreSQL :

```bash
sudo systemctl restart postgresql
```

### Mis en place du nom de domaine (certificat TLS)
doc : https://forum.mattermost.com/t/mattermost-recipe-using-lets-encrypt-for-tls-certificates-with-mattermost-docker/7596

Nous partons du principe que le nom de domaine que vous souhaitez utiliser se trouve chez Infomaniak.
  
Nous allons commencer par nous rendre sur notre compte Infomaniak : [https://login.infomaniak.com/](https://login.infomaniak.com/).

Puis allez sous la rubrique "Domaine".

![_attachments/Pasted image 20240519121030.png](_attachments/Pasted%20image%2020240519121030.png)

  
Si le domaine ne vous appartient pas mais que vous avez reçu les droits dessus, n'oubliez pas de changer votre statut en haut à gauche.

![_attachments/Pasted image 20240519121355.png](_attachments/Pasted%20image%2020240519121355.png)

Dans cet exemple, si je dois utiliser un domaine qui appartient à Mateo Cerda et qu'il m'a donné les droits dessus, il faut que je clique sur son nom. (Il apparaîtra automatiquement si la personne vous a donné des droits sur quelque chose qui lui appartient). Ensuite, je vais dans la rubrique "Domaine".

Ensuite, cliquez sur le domaine que vous voulez utiliser.

![_attachments/Pasted image 20240519121809.png](_attachments/Pasted%20image%2020240519121809.png)

Puis descendez jusqu'à trouver la rubrique "Actions" et cliquez sur l'élément suivant :

![_attachments/Pasted image 20240519122019.png](_attachments/Pasted%20image%2020240519122019.png)

Ensuite, cliquez sur "Ajouter un enregistrement".

![_attachments/Pasted image 20240519122231.png](_attachments/Pasted%20image%2020240519122231.png)

Ensuite, choisissez un enregistrement de type A.
 
Ensuite, renseignez le champ suivant avec les bonnes informations :

![_attachments/Pasted image 20240519122845.png](_attachments/Pasted%20image%2020240519122845.png)

1. Mettez ce que vous voulez dans le champ "Hôte", celui-ci correspond à l'URL que vous voulez taper pour accéder à la page de votre Mattermost.
2. Dans le champ "Valeur", mettez l'IP de votre machine (la même que vous utilisez pour vous connecter en SSH sur Mobax).
3. C'est fini ! Vous pouvez cliquer sur "Enregistrer".

Maintenant, nous allons installer le certificat sur notre machine. Pour ce faire, allez sur votre machine.

Nous allons d'abord installer Certbot sur la machine. C'est lui qui va nous permettre d'installer le certificat.

```bash
sudo apt-get install certbot
```

Ensuite, nous allons exécuter la commande suivante pour installer le certificat.

```bash
sudo certbot certonly --standalone -d mattermost.example.com
```

À la place de "mattermost.example.com", vous devez mettre le "Hôte" que vous avez renseigné dans Infomaniak. Dans mon cas, ce serait "mattermost.yttrium-band.com".

![_attachments/Pasted image 20240519125049.png](_attachments/Pasted%20image%2020240519125049.png)

À cette étape, il vous suffit juste de mettre votre adresse e-mail.

Ensuite, appuyez sur Y pour confirmer tous les choix.

![_attachments/Pasted image 20240519125155.png](_attachments/Pasted%20image%2020240519125155.png)

![_attachments/Pasted image 20240519125205.png](_attachments/Pasted%20image%2020240519125205.png)

Et voilà, le certificat est installé sur la machine. Il ne reste plus qu'à configurer le fichier de configuration de Mattermost pour le démarrer.
### Installation
doc : https://docs.mattermost.com/install/install-ubuntu.html#setup

Avant de démarrer le serveur Mattermost, vous devez modifier le fichier de configuration. Un exemple de fichier de configuration se trouve à l'adresse `/opt/mattermost/config/config.defaults.json`.

Renommez ce fichier de configuration avec les autorisations correctes :

```bash
sudo install -C -m 600 -o mattermost -g mattermost /opt/mattermost/config/config.defaults.json /opt/mattermost/config/config.json
```

Nous allons commencer à configurer ce qu'il faut dans le fichier de configuration. Pour ce faire :

```bash
sudo nano /opt/mattermost/config/config.json
```

![_attachments/Pasted image 20240519131558.png](_attachments/Pasted%20image%2020240519131558.png)

1. Mettez le "Hôte" que vous avez mis dans Infomaniak avec "https://" devant.
2. Mettez Mattermost sur le port 443 pour utiliser HTTPS (pas besoin d'ouvrir le pare-feu car nous l'avons déjà fait avec le groupe de sécurité).
3. Ici, mettez le type de certificat utilisé. Avec Certbot, c'est un certificat TLS.
4. Grâce à ce champ mis à true, Mattermost va automatiquement chercher le certificat TLS.
5. Le but de ce champ est de faire en sorte que toutes les requêtes HTTP soient redirigées en HTTPS.

Il ne reste plus que la connexion à la base de données :

![_attachments/Pasted image 20240519132551.png](_attachments/Pasted%20image%2020240519132551.png)

Vous pouvez maintenant appuyer sur "Ctrl + X" pour quitter et sur "Y" pour enregistrer.

Et lancez les commandes suivantes pour permettre à Mattermost de se lancer sur le port 443 :

```bash
cd /opt
```

```bash 
sudo setcap cap_net_bind_service=+ep ./mattermost/bin/mattermost
```

Ensuite, nous allons lancer le service Mattermost :

```bash
sudo systemctl start mattermost
```

Faites la commande suivante pour que le service Mattermost se lance à chaque démarrage de la machine :

```bash
sudo systemctl enable mattermost.service
```

Il ne vous reste plus qu'à taper l'URL que vous avez configurée pour accéder à la page de Mattermost. Pour ma part, c'est mattermost.yttrium-band.com.

Cliquez sur "Continuer dans le navigateur".

Créez-vous un compte local et suivez les instructions de configuration.

# Et bienvenue dans Mattermost !
