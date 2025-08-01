FROM node:22.12-alpine

WORKDIR /app

RUN apk add g++ make python3

ENV PORT 8080

COPY package.json .
COPY yarn.lock .
COPY tsconfig.json .
COPY .eslintrc .

COPY /packages/readabilityjs/package.json ./packages/readabilityjs/package.json
COPY /packages/import-handler/package.json ./packages/import-handler/package.json
COPY /packages/utils/package.json ./packages/utils/package.json

RUN yarn install --pure-lockfile --network-timeout=300000


ADD /packages/utils ./packages/utils
ADD /packages/import-handler ./packages/import-handler
ADD /packages/readabilityjs ./packages/readabilityjs
RUN yarn workspace @omnivore/utils build
RUN yarn workspace @omnivore/import-handler build

# After building, fetch the production dependencies
RUN rm -rf /app/packages/import-handler/node_modules
RUN rm -rf /app/node_modules
RUN yarn install --pure-lockfile --production --network-timeout=300000


EXPOSE 8080

ENTRYPOINT ["yarn", "workspace", "@omnivore/import-handler", "start"]
