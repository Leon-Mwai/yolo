
FROM node:18-alpine AS build

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm ci --production

COPY . .


FROM node:18-alpine

WORKDIR /app

COPY --from=build /usr/src/app /app

EXPOSE 6000

CMD ["node", "server.js"]
