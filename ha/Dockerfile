# Base image is the Official HAProxy
FROM haproxy:1.5

# We're to two work on this dockerfile so i change Maintainer to label with two maintainers
LABEL "maintainer1"="Steve Henriquet  <steve.henriquet@heig-vd.ch>"
LABEL "maintainer2"="Kameni Tchieko Hyacinthe Silver <hyacinthe.kamenitchieko@heig-vd.ch>"

# Install some tools
# TODO: [HB] Update to install required tool to install NodeJS
RUN apt-get update && apt-get -y install wget curl vim iputils-ping rsyslog

# Download and install S6 overlay
RUN curl -sSLo /tmp/s6.tar.gz https://github.com/just-containers/s6-overlay/releases/download/v1.17.2.0/s6-overlay-amd64.tar.gz \
  && tar xzf /tmp/s6.tar.gz -C / \
  && rm -f /tmp/s6.tar.gz

# TODO: [Serf] Install

# TODO: [HB] Install NodeJS

# TODO: [HB] Install Handlebars

# This will start S6 as our main process in our container
ENTRYPOINT ["/init"]

# TODO: [Serf] Add Serf S6 setup

# TODO: [Serf] Copy events handler scripts

# Copy the haproxy and syslog config
COPY config/haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
COPY config/rsyslogd.cfg /etc/rsyslog.d/49-haproxy.conf

# TODO: [HB] Copy the haproxy configuration template

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# TODO: [CFG] Create the nodes folder

# TODO: [Serf] Expose ports

# Expose the HA proxy ports
EXPOSE 80 1936

# Define an environment variable
ENV ROLE balancer

# Copy the S6 service and make the run script executable
COPY services/ha /etc/services.d/ha
RUN chmod +x /etc/services.d/ha/run
