FROM alpine

# Copy FFMPEG Binary
COPY ffmpeg /usr/local/bin/ffmpeg

# Copy FFPROBE Binary
COPY ffprobe /usr/local/bin/ffprobe

# FFMPEG Version
RUN ffmpeg -version

# FFPROBE Version
RUN ffprobe -version