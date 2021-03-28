# systemd+pm2

1. start the service
- > pm2 start index.js --name
2. init the startup
- > pm2 startup
3. copy the env path
4. save the config
- > pm2 save
5. (optional) check your dump
- > cat /home/user/.pm2/dump.pm2
6. At any point in the future, to update the list of process, just run pm2 save again.
