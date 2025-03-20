# To-Do
- [ ] Set up repo layout
- [ ] Add current server issues to "Issues"
- [ ] Document current server setup (see notes below)
- [x] Upload docker-compose files
- [ ] Roadmap for future plans/ideas
- [ ] Set up a way to commit/pull from GitHub

# Notes
## Current setup To-Do
- [ ] Document file/folder structure
- [ ] Make a list of all packages to add to a fresh setup
- [ ] Document OMV config for replication
- [ ] Finish organizing containers into compose files
- [ ] Clean up Portainer (stacks, networks, & volumes)

### Issues (add to issues?)
- [x] Restore wireguard config (originally stored in docker volume) - maybe use bind mount instead?
- [ ] Filebrowser isn't accessible
- [ ] Netdata v1 isn't working

## Enterprise Brain Dump
Right now I'm running OMV as the main OS, and this server is acting as a NAS as well as hosting almost all my Docker conatiners.  
In the future, I would like this machine to just be a NAS, and move everything Docker to another machine running Proxmox.  
I also need to update OMV to the latest version as I am 2-3 versions behind currently.  
To do this, I need to make a backup of my entire configuration first, so I can easily set things back up and have minimal downtime.  
My main concern is getting this back up and running as a NAS as quickly as possible so Plex can access the files on the drives.  
I have most of the Docker containers converted over to Docker compose, and once that's all finished, I can start taking notes on my OMV config.  

## Notes on configurations
### Home Assistant
In order to access HA through reverse proxy, make sure the IP address of the 'nginx-reverse-proxy' container is added to the 'configuration.yaml' file for HA.
