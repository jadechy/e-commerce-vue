FROM node:18-alpine AS build
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
RUN npm run build

FROM build AS copy
WORKDIR /usr/src/app
COPY --from=build /usr/src/app/dist ./

 
FROM copy AS base
WORKDIR /usr/src/app
COPY --from=copy /usr/src/app/ .
EXPOSE 8080
USER 1000

FROM base AS final 
ENV ENV=prod
CMD ["npm", "run", "start"] 