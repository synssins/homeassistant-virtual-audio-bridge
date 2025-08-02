ARG BUILD_FROM
FROM $BUILD_FROM

# Install required packages
RUN apk add --no-cache \
    pulseaudio \
    pulseaudio-utils \
    alsa-utils \
    python3 \
    py3-pip \
    py3-flask \
    py3-paho-mqtt \
    py3-requests \
    py3-yaml \
    bash \
    jq \
    curl \
    sox \
    ffmpeg

# Copy addon files
COPY rootfs/ /

# Make scripts executable
RUN chmod +x /usr/bin/virtual-audio-bridge \
    && chmod +x /etc/services.d/virtual-audio-bridge/run \
    && chmod +x /etc/services.d/virtual-audio-bridge/finish

# Create required directories
RUN mkdir -p /var/lib/pulseaudio \
    && mkdir -p /tmp/audio-streams \
    && mkdir -p /var/log/virtual-audio-bridge

# Set up PulseAudio
RUN echo "load-module module-native-protocol-unix auth-anonymous=1 socket=/tmp/pulse-socket" >> /etc/pulse/system.pa

# Expose default ports (configurable)
EXPOSE 8765 8766 8767 8768

# Set working directory
WORKDIR /usr/bin

# Labels
LABEL \
    io.hass.name="Virtual Audio Bridge" \
    io.hass.description="Creates virtual audio sinks that stream to network devices" \
    io.hass.type="addon"