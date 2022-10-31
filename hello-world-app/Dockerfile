FROM node:16-alpine

WORKDIR /usr/src/app
ENV NODE_ENV=production

COPY package.json yarn.lock server.js ./
RUN yarn install --immutable --immutable-cache --check-cache --production

CMD [ "node", "server.js" ]