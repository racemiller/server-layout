FROM python:3.9-buster
LABEL maintainer="Race Miller <race_miller@msn.com>"

# Update and install ffmpeg
RUN apt-get update && \
    apt-get install -y ffmpeg 

# Copy and install requirements
COPY requirements.txt requirements.txt
RUN pip install --upgrade pip
RUN pip3 install -r requirements.txt

# create abc user so root isn't used
RUN \
	groupmod -g 1000 users && \
	useradd -u 911 -U -d /config -s /bin/false abc && \
	usermod -G users abc && \
# create some files / folders
	mkdir -p /config /app /sonarr_root /logs && \
	touch /var/lock/sonarr_youtube.lock

# add volumes
VOLUME /config
VOLUME /sonarr_root
VOLUME /logs

# add local files
COPY app/ /app

# update file permissions
RUN \
    chmod a+x \
    /app/sonarr_youtubedl.py \ 
    /app/utils.py \
    /app/config.yml.template

# ENV setup
ENV CONFIGPATH /config/config.yml

CMD [ "python", "-u", "/app/sonarr_youtubedl.py" ]
