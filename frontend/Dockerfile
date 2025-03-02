ARG TYPE=dev

FROM node:18 AS build-dev
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
 
FROM build-dev AS copy-dev
WORKDIR /usr/src/app
COPY --from=build-dev /usr/src/app .



FROM node:18-alpine AS build-prod
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
RUN npm run build

FROM build-prod AS copy-prod
WORKDIR /usr/src/app
COPY --from=build-prod /usr/src/app/dist ./

FROM copy-${TYPE} AS copy

FROM copy AS base
WORKDIR /usr/src/app
COPY --from=copy-prod /usr/src/app/ .
EXPOSE 8080
USER 1000

FROM base AS final-dev
ENV ENV=dev
CMD ["npm", "run", "dev"]

FROM base AS final-prod 
ENV ENV=prod
CMD ["npm", "run", "start"]

FROM final-${TYPE} AS final