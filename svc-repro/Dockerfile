FROM node:16-alpine as dev
RUN apk add --no-cache libc6-compat tini

ARG NPM_TOKEN
ARG PACKAGE_REPO_DOMAIN

ENV NODE_ENV development
USER node

RUN mkdir /home/node/svc-repro
RUN mkdir /home/node/.yarn
RUN mkdir /home/node/.yarn/releases

RUN chown node:node /home/node/svc-repro
RUN chown node:node /home/node/.yarn
RUN chown node:node /home/node/.yarn/releases

WORKDIR /home/node

# copy workspace deps
COPY --chown=node:node .yarnrc.docker.yml                   .yarnrc.yml
COPY --chown=node:node .yarn/releases                       .yarn/releases
COPY --chown=node:node yarn.lock                            .
COPY --chown=node:node package.json                         .

# copy project deps
COPY --chown=node:node ./svc-repro/package.json            ./svc-repro
COPY --chown=node:node ./svc-repro/yarn.lock               ./svc-repro

# needed for yarn module state
RUN yarn install

WORKDIR /home/node/svc-repro
RUN yarn install