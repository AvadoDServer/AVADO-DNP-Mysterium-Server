# FROM node:10.15.3 as build-deps-wizard
# WORKDIR /usr/src/app
# COPY wizard .
# RUN yarn add react-scripts
# RUN yarn run build

FROM node:10.15.3 as builder

WORKDIR /cert
RUN wget http://iso.ava.do/my.ava.do.crt
RUN wget http://iso.ava.do/my.ava.do.key

FROM mysteriumnetwork/myst:1.25.5-alpine


RUN apk update
RUN apk add nginx jq
COPY files/start.sh /opt/start.sh
RUN chmod 755 /opt/start.sh

COPY wizard /usr/www/wizard
COPY nginx.conf /etc/nginx/nginx.conf
RUN mkdir -p /etc/nginx/certs/
COPY --from=builder /cert/my.ava.do.crt /etc/nginx/certs/
COPY --from=builder /cert/my.ava.do.key /etc/nginx/certs/

EXPOSE 443
EXPOSE 4050
EXPOSE 4051
EXPOSE 8091

ENTRYPOINT /opt/start.sh
