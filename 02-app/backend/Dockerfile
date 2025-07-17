FROM node:22.16.0-bullseye-slim AS base

WORKDIR /usr/src/app

COPY package*.json .sequelizerc ./


FROM base AS test

ENV NODE_ENV=test

RUN --mount=type=cache,target=/usr/src/app/.npm \
  npm set cache /usr/src/app/.npm && \
  npm install

RUN apt-get update && \
  apt-get install -y curl

COPY ./src ./src
COPY ./test ./test

CMD ["npm", "run", "dev"]


FROM base AS production

ENV NODE_ENV=production

RUN --mount=type=cache,target=/usr/src/app/.npm \
  npm set cache /usr/src/app/.npm && \
  npm ci --only=production

USER node

COPY --chown=node:node ./src ./src

EXPOSE 3000

CMD ["node", "src/index.js"]
