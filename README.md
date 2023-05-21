# home-netflix

Home Netflix on a Raspberry Pi 3 using Jellyfin.

## Checklist

- [ ] First POC with Jellyfin + rclone-mounted Google Drive
- [ ] Set up Sonarr, Radarr, Prowlarr and qBittorrent
- [ ] Configure rclone to delete files after sync
- [ ] Try Jellyseer

## Prerequisites

1. Create the folder structure specified in TRaSH guide:

   ```shell
   sudo mkdir /data
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

1. Follow the official Rclone
   [guide](https://rclone.org/drive/#making-your-own-client-id) to create your
   own client ID.

1. Mount Google Drive on `/data/media` in the Raspberry using Docker..

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

### Sonarr

### Radarr

### Prowlarr

### qBittorrent
