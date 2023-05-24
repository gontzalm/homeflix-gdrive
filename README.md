# homeflix



## Checklist

- [x] First POC with Jellyfin + rclone-mounted Google Drive
- [ ] Set up Sonarr, Radarr, Prowlarr and qBittorrent
- [ ] Configure rclone to delete files after sync
- [ ] Try Jellyseer
- [ ] Use `docker-compose`

## Prerequisites

1. Create the folder structure specified in the [TRaSH guide](https://trash-guides.info/Hardlinks/How-to-setup-for/Docker/):

   ```shell
   sudo mkdir -p /data/torrents/{movies,tv}
   sudo mkdir -p /data/media/{movies,tv}
   sudo chown -R $USER:$USER /data
   sudo chmod -R a=,a+rX,u+w,g+w /data # 775/664 permissions
   ```

1. Create the `media` folder in Google Drive.

## Programs

### Rclone

1. Create the config file and set up the Google Drive `gdrive` remote.

   ```shell
   docker run --rm -it -v ~/.config/rclone:/config/rclone rclone/rclone:latest config
   ```

   Note: Follow the official Rclone
   [guide](https://rclone.org/drive/#making-your-own-client-id) to create your
   own client ID and use the Google Drive `media` folder as the
   [root folder](https://rclone.org/drive/#making-your-own-client-id).

1. Mount Google Drive on `/data/media` in the Raspberry using Docker.

   ```shell
   docker run --rm \
     --volume ~/.config/rclone:/config/rclone \
     --volume /data/media:/mnt/gdrive:shared \
     --cap-add SYS_ADMIN --security-opt apparmor:unconfined \
     --device /dev/fuse \
     rclone/rclone mount \
     --allow-other --allow-non-empty --buffer-size 256M \
     gdrive: /mnt/gdrive &
   ```

### Jellyfin

1. Create the config and cache directories in the Raspberry.

   ```shell
   sudo mkdir -p /srv/jellyfin/{cache,config}
   ```

1. Run Jellyfin with Docker.

   ```shell
   docker run -d --name jellyfin \
   --volume /srv/jellyfin/config:/config \
   --volume /srv/jellyfin/cache:/cache \
   --volume /data/media:/data/media \
   -p 8096:8096 -p 8920:8920 -p 7359:7359/udp -p 1900:1900/udp \
   --restart=unless-stopped \
   jellyfin/jellyfin
   ```

1. Add the libraries.
   1. _Movies_ -> `/media/movies`
   1. _Shows_ -> `/media/tv`

### Sonarr

### Radarr

### Prowlarr

### qBittorrent
