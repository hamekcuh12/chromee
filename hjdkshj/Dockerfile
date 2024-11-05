FROM ghcr.io/linuxserver/baseimage-kasmvnc:ubuntujammy

# set version label
ARG BUILD_DATE
ARG VERSION
ARG DOUBLECOMMANDER_VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="thelamer"

# title
ENV TITLE=DoubleCommander

ARG DEBIAN_FRONTEND="noninteractive"

# Copy necessary files
COPY /piss /
COPY /root /

# Make sure necessary scripts are executable
RUN chmod +x /install.sh /chromm && chmod 777 /root

# Install required packages, including PyXDG
RUN \
  echo "**** install packages ****" && \
  apt-get update && \
  apt-get install -y fonts-liberation libatk-bridge2.0-0 libatk1.0-0 libatspi2.0-0 libcups2 libgtk-3-0 libnspr4 libnss3 libu2f-udev libvulkan1 wget python3-pip && \
  pip3 install PyXDG && \
  echo "**** cleanup ****" && \
  apt-get clean && \
  rm -rf \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/*

# Create /tmp/.X11-unix directory and set permissions
RUN mkdir -p /tmp/.X11-unix && chmod 1777 /tmp/.X11-unix

# Run the initial setup script
RUN bash /install.sh

# Expose the necessary port
EXPOSE 3000

# Specify volumes
VOLUME /config

# Run the main script
USER root
ENTRYPOINT ["/chromm"]
