# dokku-radarr

```sh
dokku apps:create radarr
dokku config:set --no-restart radarr PUID=1001 PGID=1001 TZ="America/Los Angeles" UMASK_SET=022

mkdir -p /var/lib/dokku/data/storage/radarr/config
chown -R dokku:dokku /var/lib/dokku/data/storage/radarr
mkdir -p /mnt/bucket/downloads
dokku storage:mount radarr /var/lib/dokku/data/storage/radarr/config:/config
dokku storage:mount radarr /mnt/bucket/Plex:/movies
dokku storage:mount radarr /mnt/bucket/downloads:/downloads

docker pull linuxserver/radarr
docker tag linuxserver/radarr dokku/radarr:latest
dokku tags:deploy radarr latest
dokku proxy:ports-add radarr http:80:7878
dokku config:set ghost url=http://radarr.mydomain.com

usermod -a -G storage dokku
```
