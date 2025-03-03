FROM node:18-alpine AS build 
COPY package.json ./
RUN npm install --omit=dev 
 
FROM build AS base 
COPY --from=build . .
EXPOSE 3000
USER 1000

FROM base AS final 
ENV ENV=prod 
CMD ["npm", "run", "start"] 