ARG NODE_VERSION=11
FROM arm32v7/node:${NODE_VERSION}
# add support for gpio library
RUN apt-get update
#RUN apt-get install python-rpi.gpio
RUN apt-get install -y sudo lsb-release

# Home directory for Node-RED application source code.
RUN mkdir -p /usr/src/node-red

# User data directory, contains flows, config and nodes.
RUN mkdir /data

WORKDIR /usr/src/node-red

# Add node-red user so we aren't running as root.
RUN useradd --home-dir /usr/src/node-red --no-create-home node-red \
    && chown -R node-red:node-red /data \
    && chown -R node-red:node-red /usr/src/node-red \
    && echo "node-red:node-red" | chpasswd \
    && adduser node-red sudo

RUN echo "node-red ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/node-red

USER node-red

# run the official install/upgrade script
RUN curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered > /tmp/updatescript.sh \
    && chmod u+x /tmp/updatescript.sh \
    && echo "y" | /tmp/updatescript.sh


# expose nodered port
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json
ENV NODE_PATH=/usr/src/node-red/node_modules:/data/node_modules

CMD ["node-red-pi", "--max-old-space-size=256", "--userDir", "/data"]
