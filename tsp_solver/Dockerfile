FROM debian:buster-slim as buildstage
LABEL maintainer="Utsav Mishra <https://festivitymishra.github.io/>"

# Install dependencies
RUN apt-get update && \
    apt-get install -y git pkg-config && \
    apt-get -y --no-install-recommends install cmake make git gcc g++ libbz2-dev libstxxl-dev libstxxl1v5 libxml2-dev \
        libzip-dev libboost-all-dev lua5.2 liblua5.2-dev libtbb-dev -o APT::Install-Suggests=0 -o APT::Install-Recommends=0


# Build vroom-backend

WORKDIR /

COPY vroom-feature-pickup-and-delivery vroom

RUN mkdir vroom/bin && \
    cd vroom/src && \
    make && \
    rm -rf /usr/local/lib/libosrm* && \
    cp ../bin/* /usr/local/bin && \
    strip /usr/local/bin/*

FROM debian:buster-slim as runstage


# Copy built osrm and vroom binaries from `buildstage`

COPY --from=buildstage /usr/local /usr/local 
COPY --from=buildstage /opt /opt

# Install dependencies

RUN mkdir -p /src && \
    mkdir -p /opt && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        libboost-chrono1.67.0 libboost-date-time1.67.0 libboost-filesystem1.67.0 libboost-iostreams1.67.0 \
        libboost-program-options1.67.0 libboost-regex1.67.0 libboost-thread1.67.0 liblua5.2-0 libtbb2 \
        curl \
        software-properties-common \
        git && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get update && apt-get install -y nodejs

# Build vroom-express
RUN git clone --depth 1 https://github.com/VROOM-Project/vroom-express.git && \
    useradd -m -s /bin/bash vroom && \
    mkfifo -m 600 /vroom-express/logpipe && \
    chown vroom /vroom-express/logpipe && \
    ln -sf /vroom-express/logpipe /vroom-express/access.log && \
    ln -sf /vroom-express/logpipe /vroom-express/error.log && \
    cd /vroom-express && \
    npm install && \
    apt purge -y git curl && \
    apt autoremove --purge -y && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# Copy config file
COPY config.yml /vroom-express/config.yml

WORKDIR /vroom-express

EXPOSE 3000
EXPOSE 5000

CMD ["npm", "start"]
