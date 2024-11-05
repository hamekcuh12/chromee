FROM ghcr.io/linuxserver/baseimage-kasmvnc:ubuntujammy

# Set version label
ARG BUILD_DATE
ARG VERSION
LABEL build_version="Linuxserver.io version:- ${VERSION} Build-date:- ${BUILD_DATE}"
LABEL maintainer="thelamer"

# Title
ENV TITLE=DoubleCommander

# Set non-interactive mode for apt
ARG DEBIAN_FRONTEND="noninteractive"

# Copy necessary files
COPY /piss /piss  # Copy the entire piss folder
COPY /root /root

# Ensure scripts are executable
RUN chmod +x /piss/install.sh /chromm && chmod +x /root/*

# Install required packages, including Google Chrome
RUN \
  echo "**** install packages ****" && \
  apt-get update && \
  apt-get install -y \
    fonts-liberation \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libatspi2.0-0 \
    libcups2 \
    libgtk-3-0 \
    libnspr4 \
    libnss3 \
    libu2f-udev \
    libvulkan1 \
    wget \
    google-chrome-stable && \  # Ensure Chrome is installed
  echo "**** cleanup ****" && \
  apt-get clean && \
  rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

# Create /tmp/.X11-unix directory and set permissions
RUN mkdir -p /tmp/.X11-unix && chmod 1777 /tmp/.X11-unix

# Run the initial setup script
RUN bash /piss/install.sh  # Run install.sh from the piss directory

# Expose the necessary port
EXPOSE 3000

# Specify volumes
VOLUME /config

# Use a non-root user to run Chrome (if applicable)
# Create a user (you can choose any username)
RUN useradd -ms /bin/bash chromeuser
USER chromeuser

# Set the default command to run your application
ENTRYPOINT ["/chromm"]
