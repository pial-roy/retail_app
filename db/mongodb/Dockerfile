# Use an appropriate base image for Raspberry Pi
FROM arm32v7/ubuntu:focal  # Change to arm64v8/ubuntu:focal if using 64-bit OS

# Set environment variables
ENV MONGO_VERSION=4.4.6

# Install necessary packages
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
    wget \
    gnupg \
    ca-certificates \
    jq \
    numactl \
    passwd \
    && rm -rf /var/lib/apt/lists/*

# Create mongodb user and group
RUN set -eux; \
    groupadd -g 999 mongodb || true; \
    useradd -u 999 -g mongodb -m -d /data/db mongodb || true; \
    mkdir -p /data/db /data/configdb; \
    chown -R mongodb:mongodb /data/db /data/configdb

# Download MongoDB binaries
RUN set -eux; \
    wget -O "/tmp/unofficial-mongodb.ce.pi.${MONGO_VERSION}.tar.gz" \
    "https://github.com/themattman/mongodb-raspberrypi-binaries/releases/download/${MONGO_VERSION}-rpi-unofficial/mongodb.ce.pi.${MONGO_VERSION}.tar.gz" \
    && tar xzvf "/tmp/unofficial-mongodb.ce.pi.${MONGO_VERSION}.tar.gz" -C /tmp \
    && mv /tmp/mongo /usr/local/bin \
    && mv /tmp/mongod /usr/local/bin \
    && mv /tmp/mongos /usr/local/bin \
    && rm /tmp/README.md \
    && rm /tmp/LICENSE-Community.txt

# Install the required libstdc++
RUN set -eux; \
    wget -O "/tmp/libstdc++.so.6.0.29" "https://github.com/themattman/raspberrypi-binaries/raw/main/libstdc%2B%2B/libstdc%2B%2B.so.6.0.29" \
    && mv "/tmp/libstdc++.so.6.0.29" "/usr/local/lib/libstdc++.so.6.0.29" \
    && ln -sf "/usr/local/lib/libstdc++.so.6.0.29" "/usr/lib/aarch64-linux-gnu/libstdc++.so.6"

# Expose MongoDB port
EXPOSE 27017

# Start MongoDB service
USER mongodb
CMD ["mongod", "--bind_ip", "0.0.0.0", "--dbpath", "/data/db", "--replSet", "rs0"]