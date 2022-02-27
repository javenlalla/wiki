# My Wiki

This application serves my personal wiki as powered by [BookStack](https://www.bookstackapp.com/).

## Docker Image

The Docker Image used to power this application is supplied by [LinuxServer.io](https://www.linuxserver.io/).

<https://hub.docker.com/r/linuxserver/bookstack>

## Setup

1. Copy `.env.sample` to `.env` and update the targeted values.
1. Initialize the containers and start the application by executing the following command: `docker-compose up -d`

## Backup And Restore

The following sections outline how to execute backups and restores of the application's data which includes files, assets, and the database.

If performing a restore or migration, follow the steps in the [Setup](#setup) section first and ensure the containers are running before restoring the data.

### Database Backup

```bash
docker exec -it bookstack_db mysqldump -u root -pPASSWORD bookstackapp | gzip > backups/db/db.$(date +%Y-%m-%d).sql.gz
```

### Content Backup

```bash
docker exec -it bookstack tar -czvf //backups//content.$(date +%Y-%m-%d).tar.gz //var//www//html//.env //config//www//uploads //config//www//files //config//www//images
```

Note: The double slashes are used when executing from a Windows host into the Container. See below for an example of a command without using the double slashes.

```bash
docker exec -it bookstack bash -c "tar -czvf /backups/content.$(date +%Y-%m-%d).tar.gz /var/www/html/.env /config/www/uploads /config/www/files /config/www/images"
```

### Database Restore

```bash
docker exec -it bookstack_db bash -c "gunzip < /backups/db.BACKUP_DATE.sql.gz | mysql -u root -pPASSWORD bookstackapp"
```

### Content Restore

```bash
docker exec -it bookstack tar -xvzf content.BACKUP_DATE.tar.gz
docker exec -it bookstack bash -c "cp -R config/www/files/* /config/www/files/"
docker exec -it bookstack bash -c "cp -R config/www/images/* /config/www/images/"
docker exec -it bookstack bash -c "cp -R config/www/uploads/* /config/www/uploads/"
docker exec -it bookstack bash -c "cp var/www/html/.env /var/www/html/.env"
```

## Upgrade

In order to upgrade the BookStack instance, create a backup and then execute the following commands to upgrade the instance within the current containers.

```bash
sudo docker-compose pull
sudo docker-compose up -d
```

## Additional Commands

### Connecting To Containers

```bash
sudo docker exec -it bookstack bash
sudo docker exec -it bookstack_db bash
```

### Executing Commands In Containers

```bash
docker exec -it bookstack_db mysql --version
```
