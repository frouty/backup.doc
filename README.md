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
  - make install -j4 
- 4 To automatically start UrBackup on jail boot: TODO 

    echo "#!/bin/sh" > /etc/rc.local  
    echo "/usr/local/bin/urbackupsrv run -d -g 104857600 -u root" >> /etc/rc.local  
    chmod +x /etc/rc.local  
 "/usr/local/bin/urbackupsrv run -d -g 104857600 -u root" cette commande ne marche pas pour faire demarrer le service.  
 /usr/local/bin/urbackupsrv run -d marche. pour faire demarrer le service.
 
- 5 Restart the jail.

- 6 Browse to http://jail-ip:55414 and configure an admin user and the backup storage path.
