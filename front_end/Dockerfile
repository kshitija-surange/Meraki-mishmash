FROM node:slim
LABEL maintainer="Utsav Mishra <https://festivitymishra.github.io/>"

WORKDIR /

RUN apt-get update && \
    apt-get install -y git && \
    git clone --depth 1 https://github.com/VROOM-Project/vroom-frontend.git && \
    cd /vroom-frontend && \
    npm install && \
    apt purge -y git && \
    apt autoremove --purge -y && \
    apt clean && \
    apt-get autoremove -y && \
    apt-get autoclean

COPY vroom.svg /vroom-frontend/images/vroom.svg
COPY panel.js /vroom-frontend/src/controls/panel.js

COPY api.js /vroom-frontend/src/config/api.js
COPY leaflet_setup.js /vroom-frontend/src/config/leaflet_setup.js

WORKDIR /vroom-frontend

EXPOSE 9966

CMD ["npm", "run", "serve"]
