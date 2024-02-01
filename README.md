# homeflix-gdrive

> Archived: Due to Google Drive storage limitations, I no longer use this
> architecture to host my home Netflix. I now use an HDD bay and the
> configuration is in the [homeflix](https://github.com/gontzalm/homeflix)
> repository.

## Hardware

- Raspberry Pi 4B 8GB
- 1TB NVMe SSD

## Prerequisites

1. Install Arch Linux ARM (`aarch64`) on the SSD following the guides

   - [Raspberry Pi 4 - Arch Linux ARM](https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-4)
   - [Arch Linux ARM USB Boot on Raspberry Pi 4](https://gist.github.com/yogaxpto/9495fb9f76a6321aa483e0f73d4ffaf2)
   - This [comment](https://gist.github.com/yogaxpto/9495fb9f76a6321aa483e0f73d4ffaf2?permalink_comment_id=4206041#gistcomment-4206041) on the previous gist

1. Create the folder structure specified in the
   [TRaSH guide](https://trash-guides.info/Hardlinks/How-to-setup-for/Docker/):

   ```shell
   sudo mkdir -p /data/torrents/{movies,tv}
   sudo mkdir -p /data/media/{movies,tv}
   sudo chown -R $USER:$USER /data
   sudo chmod -R a=,a+rX,u+w,g+w /data # 775/664 permissions
   ```

1. Create the `media` folder in Google Drive.

## Setup

### Rclone

1. Create the directories required by the Rclone Docker plugin.

   ```shell
   sudo mkdir -p /var/lib/docker-plugins/rclone/{config,cache}
   ```

1. Install the Rclone Docker plugin.

   ```shell
   docker plugin install rclone/docker-volume-rclone:arm-v7 \
     --grant-all-permissions --alias rclone
   ```

1. Create the config file and set up the Google Drive `gdrive` remote.

   ```shell
   docker run --rm -it \
     -v /var/lib/docker-plugins/rclone/config:/config/rclone \
     rclone/rclone:latest config
   ```

   Note: Follow the official Rclone
   [guide](https://rclone.org/drive/#making-your-own-client-id) to create your
   own client ID and use the Google Drive `media` folder as the
   [root folder](https://rclone.org/drive/#making-your-own-client-id).

### Sonarr

1. Create the config directory.
   ```shell
   sudo mkdir -p /srv/sonarr/config
   ```

### Radarr

1. Create the config directory.
   ```shell
   sudo mkdir -p /srv/radarr/config
   ```

### Prowlarr

1. Create the config directory.

   ```shell
   sudo mkdir -p /srv/prowlarr/config
   ```

### qBittorrent

1. Create the config directory.

   ```shell
   sudo mkdir -p /srv/qbittorrent/config

   ```

### Bazarr

1. Create the config directory.

   ```shell
   sudo mkdir -p /srv/bazarr/config
   ```

### Jellyfin

1. Create the config and cache directories.

   ```shell
   sudo mkdir -p /srv/jellyfin/{cache,config}
   ```

1. Access the web client and add the libraries.
   1. _Movies_ -> `/media/movies`
   1. _Shows_ -> `/media/tv`
