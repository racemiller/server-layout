## To update yt-dlp for the container
1. cd to /home/race/sonarr-yt-dlp-test
2. Update "requirements.txt" to match the latest version of yt-dlp
3. Run command "docker build -t sonarr-ytdlp:latest ."
4. Clean up old docker images "docker image prune"

## Run container once
1. Run "docker compose run -rm sonarr-ytdlp-test" and let it run, then hit Ctl-C to end the process and remove the container when it's done.