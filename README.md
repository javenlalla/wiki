# My Wiki

This application serves my personal wiki as powered by [BookStack](https://www.bookstackapp.com/).

## Docker Image

The Docker Image used to power this application is supplied by [LinuxServer.io](https://www.linuxserver.io/).

<https://hub.docker.com/r/linuxserver/bookstack>

## Database backup

`@TODO`: gzip the backup file.
sudo docker exec -it bookstack_db bash
container: time mysqldump -u root -pPASSWORD bookstackapp > /config/backup.sql

## Content backup

sudo docker exec -it bookstack bash
container: cd /var/www/html
container: time tar -czvf /config/bookstack-files-backup.tar.gz .env public/uploads storage/uploads -h
time tar -czvfh /config/bookstack-files-backup3.tar.gz .env public/uploads storage/uploads

time tar -czvf /config/bookstack-files-backup2.tar.gz .env /config/www/uploads /config/www/files /config/www/images

/config/www/uploads
/config/www/files
/config/www/images

## Migration

1. Copy `docker-compose.yml` file to target server.

   - Update the `APP_URL` as necessary.

1. `docker-compose up -d`
1. Execute Database and Content restore according to the following sections.
1. Restart containers. `docker-compose stop` then `docker-compose up -d`

### Database Restore

time mysql -u root -pPASSWORD bookstackapp < backup.sql

### Content Restore

```bash
time tar -xvzf bookstack-files-backup2.tar.gz
cp -R config/www/files/* /config/www/files/
cp -R config/www/images/* /config/www/images/
cp -R config/www/uploads/* /config/www/uploads/
cp .env /var/www/html/.env
```

## Upgrade

sudo docker-compose pull
sudo docker-compose up -d

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
