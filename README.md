# backup.doc

# controle de l'état des disques durs
## smartcontrol
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
