FROM arm32v7/debian:bookworm-slim

# Update apt cache and install prerequisites
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y gpg iputils-ping \
  ca-certificates apt-utils openssl

# Add Flightradar24 keys
RUN KEY_ID=C969F07840C430F5 && apt-key adv --recv-key --keyserver keyserver.ubuntu.com ${KEY_ID} || \
apt-key adv --recv-key --keyserver pool.sks-keyservers.net ${KEY_ID} || \
apt-key adv --recv-key --keyserver pgp.mit.edu ${KEY_ID} && \
echo 'deb http://repo.feed.flightradar24.com flightradar24 raspberrypi-stable' >> /etc/apt/sources.list


# Create dummy systemctl, so install of fr24feed will not fail
RUN echo '#!/bin/sh' > /bin/systemctl && chmod 755 /bin/systemctl

# Update apt cache and install fr24feed
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y fr24feed

# Copy fr24feed configuration
COPY assets/fr24feed.ini /etc/fr24feed.ini

# Copy startup script
COPY assets/flightradar24.sh /usr/bin/flightradar24.sh
RUN chmod 755 /usr/bin/flightradar24.sh

# Expose web dashboard
EXPOSE 8754

# Container start point
CMD ["/usr/bin/flightradar24.sh"]
