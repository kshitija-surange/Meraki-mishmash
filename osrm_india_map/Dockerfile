FROM debian:buster-slim as buildstage
LABEL maintainer="Utsav Mishra <https://festivitymishra.github.io/>"

# Install dependencies
RUN apt-get update && \
    apt-get install -y git pkg-config && \
    apt-get install wget && \
    apt-get -y --no-install-recommends install cmake make git gcc g++ libbz2-dev libstxxl-dev libstxxl1v5 libxml2-dev \
        libzip-dev libboost-all-dev lua5.2 liblua5.2-dev libtbb-dev -o APT::Install-Suggests=0 -o APT::Install-Recommends=0

# Build osrm-backend
RUN git clone -b v5.19.0 --depth 1 https://github.com/Project-OSRM/osrm-backend.git && \
    cd osrm-backend && \
    echo "Building OSRM ${DOCKER_TAG}" && \
    git show --format="%H" | head -n1 > /opt/OSRM_GITSHA && \
    echo "Building OSRM gitsha $(cat /opt/OSRM_GITSHA)" && \
    mkdir -p build && \
    cd build && \
    BUILD_TYPE="Release" && \
    ENABLE_ASSERTIONS="Off" && \
    BUILD_TOOLS="Off" && \
    case ${DOCKER_TAG} in *"-debug"*) BUILD_TYPE="Debug";; esac && \
    case ${DOCKER_TAG} in *"-assertions"*) BUILD_TYPE="RelWithDebInfo" && ENABLE_ASSERTIONS="On" && BUILD_TOOLS="On";; esac && \
    echo "Building ${BUILD_TYPE} with ENABLE_ASSERTIONS=${ENABLE_ASSERTIONS} BUILD_TOOLS=${BUILD_TOOLS}" && \
    cmake .. -DCMAKE_BUILD_TYPE=${BUILD_TYPE} -DENABLE_ASSERTIONS=${ENABLE_ASSERTIONS} -DBUILD_TOOLS=${BUILD_TOOLS} -DENABLE_LTO=On && \
    make install && \
    cd .. && \
    wget -O india.osm.pbf --random-wait http://download.geofabrik.de/asia/india-latest.osm.pbf && \
    osrm-extract india.osm.pbf -p profiles/car.lua && \
    osrm-partition india.osrm  && \
    osrm-customize india.osrm  && \
    rm -r india.osm.pbf && \
    cd build && \
    cd ../profiles && \
    cp -r * /opt && \
    ldconfig && \
    cd ..

WORKDIR /osrm-backend

CMD osrm-routed --max-matching-size 50000 --max-table-size 50000 --max-viaroute-size 50000 --algorithm=MLD india.osrm 

