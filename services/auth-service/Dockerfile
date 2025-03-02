ARG TYPE=dev

FROM node:18 AS build-dev
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install

FROM node:18-alpine AS build-prod
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install --omit=dev 


FROM build-dev AS copy-dev
COPY --from=build-dev /usr/src/app /usr/src/app

FROM build-prod AS copy-prod
COPY --from=build-prod /usr/src/app /usr/src/app

FROM copy-${TYPE} AS copy

FROM copy AS base
COPY . .
EXPOSE 3001
USER 1000

FROM base AS final-dev
ENV ENV=dev
CMD ["npm", "run", "dev"]

FROM base AS final-prod 
ENV ENV=prod 
CMD ["npm", "run", "start"]

FROM final-${TYPE} AS final