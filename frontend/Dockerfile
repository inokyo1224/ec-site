# Base image
FROM node:21-bookworm-slim AS base

RUN apt-get update && \
    apt-get install -y locales curl

RUN locale-gen ja_JP.UTF-8

RUN localedef -f UTF-8 -i ja_JP ja_JP

ENV LANG=ja_JP.UTF-8

ENV TZ=Asia/Tokyo

ENV NODE_ENV development

USER node

# Create app directory
FROM base AS build
WORKDIR /usr/src/app

USER root
RUN mkdir /usr/src/app/node_modules && chown -R node:node /usr/src/app/node_modules

USER node

COPY --chown=node:node package*.json ./

RUN npm ci

COPY --chown=node:node . .

USER node

EXPOSE 3000

CMD ["npm", "run", "dev"]


FROM build AS production


# Hosting in nginx
#FROM nginx:latest AS production

#RUN mkdir /var/www
#COPY --from=build /usr/src/app /var/www
#COPY nginx.conf /etc/nginx/conf.d/default.conf

