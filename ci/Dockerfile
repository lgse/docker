FROM lgse/ffmpeg:4.2.1 AS ffmpeg
FROM lgse/php:7.4.7

# Fixes some weird terminal issues such as broken clear / CTRL+L
ENV TERM=linux

# Ensure apt doesn't ask questions when installing stuff
ENV DEBIAN_FRONTEND=noninteractive

# Update packages
RUN apt-get update -y --fix-missing

# Install git
RUN apt-get install -y --no-install-recommends \
    git

# Add Repositories
RUN echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C \
    && curl https://download.docker.com/linux/ubuntu/gpg -o docker_signing.key --insecure \
    && apt-key add docker_signing.key

# Install Containerd
RUN curl -O https://download.docker.com/linux/ubuntu/dists/bionic/pool/edge/amd64/containerd.io_1.2.2-3_amd64.deb \
    && apt install ./containerd.io_1.2.2-3_amd64.deb

# Install Docker CE
RUN apt-get update -y --fix-missing \
    && apt-get install -y --no-install-recommends docker-ce

# Install FFMPEG
COPY --from=ffmpeg /usr/local/bin/ffmpeg /usr/local/bin/ffmpeg
COPY --from=ffmpeg /usr/local/bin/ffprobe /usr/local/bin/ffprobe

# Install Node
RUN curl -sL https://deb.nodesource.com/setup_14.x -o node_setup.sh \
    && bash node_setup.sh \
    && apt-get install -y nodejs

# Install PNPM
RUN npm install -g pnpm

# Clean Up
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /var/cache/apk/*

# See PHP Version
RUN php -v

# See Composer Version
RUN composer --version

# See Node Version
RUN node -v

# See FFmpeg & FFprobe Versions
RUN ffmpeg -version \
    && ffprobe -version

# See Git version
RUN git --version