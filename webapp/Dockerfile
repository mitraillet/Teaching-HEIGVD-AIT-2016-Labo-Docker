# The base image is one of the offical one
FROM node:0.12.2-wheezy

# We're to two work on this dockerfile so i change Maintainer to label with two maintainers
LABEL "maintainer1"="Steve Henriquet  <steve.henriquet@heig-vd.ch>"
LABEL "maintainer2"="Kameni Tchieko Hyacinthe Silver <hyacinthe.kamenitchieko@heig-vd.ch>"

# Install the required tools to run our webapp and some utils
RUN apt-get update && apt-get -y install wget curl vim iputils-ping && npm install -g bower

# We copy the application and make sure the dependencies are installed before
# other operations. Doing so will reduce the time required to build this image
# as downloading NPM dependencies can be quite long.
COPY app /backend/app
RUN cd /backend/app && npm install && bower install --allow-root

# Download and install S6 overlay
RUN curl -sSLo /tmp/s6.tar.gz https://github.com/just-containers/s6-overlay/releases/download/v1.17.2.0/s6-overlay-amd64.tar.gz \
  && tar xzf /tmp/s6.tar.gz -C / \
  && rm -f /tmp/s6.tar.gz

# TODO: [Serf] Install

# This will start S6 as our main process in our container
ENTRYPOINT ["/init"]

# TODO: [Serf] Add Serf S6 setup

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# TODO: [Serf] Expose ports

# Expose the web application port
EXPOSE 3000

# Define an environment variable for the role of the container
ENV ROLE backend

# Copy the S6 service and make the run script executable
COPY services/node /etc/services.d/node
RUN chmod +x /etc/services.d/node/run
