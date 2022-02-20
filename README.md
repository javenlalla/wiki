# My Wiki

This application serves my personal wiki as powered by [BookStack](https://www.bookstackapp.com/).

## Docker Image

The Docker Image used to power this application is supplied by [LinuxServer.io](https://www.linuxserver.io/).

<https://hub.docker.com/r/linuxserver/bookstack>

## Setup

1. Copy `.env.sample` to `.env` and update targeted values.
1. Initialize the containers and application by executing: `docker-compose up -d`

## Database Backup

```sql
docker exec -it bookstack_db mysqldump -u root -pPASSWORD bookstackapp | gzip > backups/db/$(date +%Y-%m-%d).sql.gz
```

## Content Backup

sudo docker exec -it bookstack bash
container: cd /var/www/html
container: time tar -czvf /config/bookstack-files-backup.tar.gz .env public/uploads storage/uploads -h
time tar -czvfh /config/bookstack-files-backup3.tar.gz .env public/uploads storage/uploads

time tar -czvf /config/bookstack-files-backup2.tar.gz .env /config/www/uploads /config/www/files /config/www/images

/config/www/uploads
/config/www/files
/config/www/images

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
