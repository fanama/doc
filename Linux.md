# Commandes linux

## Personalisation

- nom terminal

copy the following command to `~./bashrc`

- > export PS1='\u> '

## file

- lire un fichier

    - > tail -60 filepath

- download

    - > curl -O url
 
## Ports

- lister les Ports : 

    - > sudo lsof -i -P -n | grep LISTEN

- ouvrir un port : 

    - > sudo ufw allow 2222
## Alias

    - > alias nom_de_lalias='commande'

## SQL

- connexion : 

    - > mysql -h 10.8.3.34 -P 3302 -u root -p

## Deployement

- pm2

    - > pm2 start app.js --name my-api # Name process
