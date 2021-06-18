# Mysql

1. Install

- > sudo apt install mariadb-server

2. Grant auth

```sh
CREATE USER 'paterne_gaye'@'%' IDENTIFIED BY 'mot-de-passe';
```

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'paterne_gaye'@'%' IDENTIFIED BY 'paterne_gaye'  WITH GRANT OPTION;
```
