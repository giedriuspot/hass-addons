ARG BUILD_FROM
FROM node:18-alpine3.15

ENV LANG C.UTF-8

RUN apk update
RUN apk add --no-cache  python3 py3-pip alpine-sdk libcap bluez bluez-dev eudev-dev

RUN \
    set -x \
    && apk add --no-cache \
        bash \
        bind-tools \
        ca-certificates \
        curl \
        jq \
        libstdc++ \
        tzdata \
        xz
		
RUN \
    mkdir -p /usr/src/bashio \
    && curl -L -f -s "https://github.com/hassio-addons/bashio/archive/v0.16.2.tar.gz" \
        | tar -xzf - --strip 1 -C /usr/src/bashio \
    && mv /usr/src/bashio/lib /usr/lib/bashio \
    && ln -s /usr/lib/bashio/bashio /usr/bin/bashio

RUN apk update
RUN apk add --no-cache npm python3 py3-pip alpine-sdk libcap bluez bluez-dev eudev-dev
#RUN npm install -g npm@8.9
RUN echo "!!!!!!PYTHON3 VERSION!!!!!!"
RUN echo python3 -v
RUN setcap cap_net_raw+eip $(eval readlink -f $(which node))

WORKDIR /

COPY start.sh /app/start.sh
COPY addon /app
RUN rm -rf /app/node_modules

RUN node -v
RUN python3 -v

RUN cd /app && npm i

# here we should just copy the /app contents to a run image with nodejs and other requirements only

ENTRYPOINT ["/app/start.sh"]

LABEL io.hass.version="VERSION" io.hass.type="addon" io.hass.arch="armhf|aarch64|i386|amd64"


