# Commandes linux

## file

- lire un fichier

    - > tail -60 filepath

## Ports

- lister les Ports : 

    - > sudo lsof -i -P -n | grep LISTEN

- ouvrir un port : 

    - > sudo ufw allow 2222

## SQL

- connexion : 

    - > mysql -h 10.8.3.34 -P 3302 -u root -p

## Deployement

- pm2

    - > pm2 start app.js --name my-api # Name process
