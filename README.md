# backup.doc

# controle de l'état des disques durs

## smartcontrol
`aptitude install smartmontools`  
- *smartd* demon qui repure info du disque à intervalle régulier
- *smartcl* qui interroge les données pour les visualiser.

- `smartcl -a /dev/sdX` donne des informations sur le disque

- `smartctl -q errorsonly -H -l selftest /dev/sdX` bref donne uniquement des infos si error.

- On peut faire des tests:
  - `smartctl -t short /dev/sdX`
  - `smartctl -t long /dev/sdX`
  Il faut attendre que le test se termine. Et il n'y pas d'indication
  - visualisation des résultats du test.`smartctl -l selftest /dev/sdX`
# comment on peut entrer dans une jail  en ssh
## par un ssh sur le freenas
- puis jls
- puis jexec JID tcsh 
## ou directement ssh dans la jail
ssh username@ipjail  
ipjail se trouve dans le webgui du freenas / jail  
Mais avant il faut voir : https://doc.freenas.org/11/jails.html

# installation de urbackup sur une freebsd 
## freebsd version
## https://www.urbackup.org/freenasserverinstall.html
- 1 Create a new FreeBSD jail (the default). 
- 2 Map the backup storage into the jail. Je n'ai pas compris ce qu'il fallait faire.
- 3 Open a shell/console to the new jail
  - pkg update
  - pkg install cryptopp
  - curl -O https://hndl.urbackup.org/Server/2.2.11/urbackup-server-2.2.11.tar.gz
  - tar xf urbackup-server-2.2.11.tar.gz
  - cd urbackup-server-2.2.11
  - ./configure
  - make install -j4 . Cela a été assez rapide pour moi.
- 4 To automatically start UrBackup on jail boot: TODO 

    echo "#!/bin/sh" > /etc/rc.local  
    echo "/usr/local/bin/urbackupsrv run -d -g 104857600 -u root" >> /etc/rc.local  
    chmod +x /etc/rc.local  
 "/usr/local/bin/urbackupsrv run -d -g 104857600 -u root" cette commande ne marche pas pour faire demarrer le service.  
 /usr/local/bin/urbackupsrv run -d marche. pour faire demarrer le service.
 
- 5 Restart the jail.

- 6 Browse to http://jail-ip:55414 and configure an admin user and the backup storage path.


https://forums.freenas.org/index.php?threads/install-urbackup-in-a-jail.22117/

## Création du dataset pour le stockage du backup
- 1 se connecter en webgui sur le freenas
- 2 dans la colonne de gauche : storage / create dataset
- 3 Dataset name : Backups
- 4 Je vois apparaitre dans /mnt/mnemosys/. /mnt/mnemosys/Backup est vide.
- 5 webgui / jail /urbackup / Add storage 
- 6 source = /mnt/mnemosys/Backups
- 7 destination = /mnt/mnemosys/jail/urbackups/mnt/Backups 
- 8 et j'ai bien dans la jail urbackpup /mnt/Backups qui est vide.
- 9 dans une console de la jail urbackup chown -R urbackup:urbackup /mnt/Backups
- 10 dans le webgui du jail urbackup
    - Go in the settings / general / server tab
    - In the [backup storage path] field, the default value is "c:/backup", change it into "/mnt/backups
 
 Maintenant il faut installer le client
 
 ## installation d'un client sur une debian. Install client on Debian or Ubuntu from sources
 
 libcrypto++-dev
 libwxgtk3.0-dev
 build-essential
 g++ je ne le trouve pas avec aptitude.
 libz-dev
 
- 1 Install the dependencies UrBackup needs: WxWidgets >= 2.9.0 On Debian/Ubuntu you can do that with apt or your favourite package manager:

apt install build-essential "g++" libwxgtk3.0-dev libcrypto++-dev libz-dev

- 2

Download the UrBackup client source files and extract them via e.g.

wget https://hndl.urbackup.org/Client/2.2.5/urbackup-client-2.2.5.tar.gz
tar xzf urbackup-client-2.2.5.tar.gz

- 3

Build the UrBackup client and install it:

cd urbackup-client-2.2.5  
./configure  
make -j4 C'est assez long.  
sudo make install  

- 4

Make sure that the UrBackup client backend runs correctly:

sudo urbackupclientbackend -v info

- 5

Start the UrBackup client backend on startup by adding it e.g. to rc.local:

sudo chmod +x /etc/rc.local
editor /etc/rc.local

Now add /usr/local/sbin/urbackupclientbackend -d before the exit 0.
6.

Start the UrBackup client frontend and setup your paths by executing

urbackupclientgui

and clicking on the tray icon and add paths. You can also do that on the server.

## urbackupclientgui 
ne marche pas tres bien.  
J'ai fait un reboot du client et cela marche.

## urbackupclientctl 
ne marche pas tres bien
## debugging 
gdb --args urbackupclientbackend -v debug  
run 
Si cela crash : bt

On peut aussi faire un tail -f /var/log/urbackup.log

## add backup path
On peut ajouter une chemin par defaut pour tous les clients. Dans le webgui du server  
settings / client settings / Default directory to backup   
Mais c'est général à tous les clients.  

## bouton droit de l'icone dans le tray.
add/remove backup path.
apres avoir un certain nombre de manoeuvre les backups ne démarrent pas et restent en queue. Reboot du serveur.

## comment effacer des backups qui sont dans le server.

