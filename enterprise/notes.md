# To-Do
- [ ] Set up repo layout
- [ ] Add current server issues to "Issues"
- [ ] Document current server setup (see notes below)
- [x] Upload docker-compose files
- [ ] Roadmap for future plans/ideas
- [ ] Set up a way to commit/pull from GitHub
- [x] Self hosted TV channels
- [x] Self hosted Radio stations

# Notes
## Current setup To-Do
- [ ] Document file/folder structure
- [ ] Make a list of all packages to add to a fresh setup
- [ ] Document OMV config for replication
- [ ] Finish organizing containers into compose files
- [ ] Clean up Portainer (stacks, networks, & volumes)
- [ ] Set up Git to handle local versioning & updating of GitHub repo

### Issues (add to issues?)
- [x] Restore wireguard config (originally stored in docker volume) - maybe use bind mount instead?
- [x] Filebrowser isn't accessible
- [ ] Netdata v1 isn't working
- [ ] Maybe run soularr (or at least xmas soularr) manually? Like as a Python script instead of a Docker container. [soularr GitHub](https://github.com/mrusse/soularr/tree/main)
- [x] Move Nextcloud to admin stack (ensure static IP for database & update config)

## Enterprise Brain Dump
Right now I'm running OMV as the main OS, and this server is acting as a NAS as well as hosting almost all my Docker conatiners.  
In the future, I would like this machine to just be a NAS, and move everything Docker to another machine running Proxmox.  
I also need to update OMV to the latest version as I am 2-3 versions behind currently.  
To do this, I need to make a backup of my entire configuration first, so I can easily set things back up and have minimal downtime.  
My main concern is getting this back up and running as a NAS as quickly as possible so Plex can access the files on the drives.  
I have most of the Docker containers converted over to Docker compose, and once that's all finished, I can start taking notes on my OMV config.  

## Notes on configurations
### Home Assistant
In order to access HA through reverse proxy, make sure the IP address of the `nginx-reverse-proxy` container is added to the `configuration.yaml` file for HA.
### Sonarr-YouTubeDL
I pulled the latest version from GitHub [sonarr_youtubedl](https://github.com/whatdaybob/sonarr_youtubedl) which has a super outdated version of yt-dlp, and built a custom Docker image based off of that. All the files are currently in `~/sonarr-ytdlp-test` (may move in the future). If you need to make any changes, update the DockerFile in there, then re-run `docker build -t [updated image name:tag] .`, then the image will get stored locally and will be accessible by docker. If you change the name or tag, make sure to update the image in the downloaders compose file.
Currently disabling this container in the docker-compose file because of ongoing issues. It performed its duty and helped me find some episodes and I don't think I need it constantly running right now anyway. Might be another good one to do a one-off run once in a while, but right now it's more work than it's worth.
### Docker Hostnames
If you add the `hostname:` value to your compose file, you can reference that container by its hostname from another container on the same network. Look at the `nextcloud` & `nextcloud_db` containers for example. This makes it so you don't need to worry about setting static IPs for containers that need to reference each other.
### TubeArchivist Plex Watch State
Currently using this Tautulli notification plugin to sync videos watched on Plex to TubeArchivist: [Tautulli Notify TubeArchivist of Plex Watched State
](https://github.com/tangyjoust/Tautulli-Notify-TubeArchivist-of-Plex-Watched-State/tree/main)
### TubeArchivist Plex Integration
This is what syncs videos in TubeArchivist to your Plex server. Currently using this script: [tubearchivist-plex](https://github.com/tubearchivist/tubearchivist-plex)
If you notice YT videos not showing up on Plex, check the above repo for an update. To update: Download the zip file from the repo, or click [here](https://github.com/tubearchivist/tubearchivist-plex/archive/refs/heads/main.zip). Cd into the Plex config folder and wget the zip file to there, then unzip it. 
Move the `Scanners/Series/TubeArchivist Series Scanner.py` file to the same location within the Plex config folder (replacing the old one).
Move the `Contents` folder to the `Plug-ins/TubeArchivist-Agent.bundle` folder, replacing the old one.
Restart Plex.
You can also check the above repo link for more instructions/directions/troubleshooting.
### Readarr
Readarr was retired on 6/27/25. Using [rreading-glasses](https://github.com/blampe/rreading-glasses) as a crutch for now. May need to switch to [LazyLibrarian](https://lazylibrarian.gitlab.io/) in the future. Also looking into [Calibre](https://calibre-ebook.com/) & [Calibre-web](https://github.com/janeczku/calibre-web) as a frontend for ebooks.
### YouTube Plex Metadata Agent
For downloaded YT videos (not through TubeArchivist), I am using the Plex plug-in [YouTube-Agent.bundle](https://github.com/ZeroQI/YouTube-Agent.bundle) which adds a metadata agent for YT videos. When you download the videos, use the command `yt-dlp --write-thumbnail --write-info-json --write-subs --format-sort "ext,codec:vp9"` to make sure the thumbnail and metadata files get downloaded so the plugin doesn't have to constantly search on YT for this info. There are better ways to use the command to organize the downloaded files, but this is working for now. Currently using it for the Kids YouTube library on Plex.
