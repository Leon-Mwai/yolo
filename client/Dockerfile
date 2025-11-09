
FROM node:18-alpine AS build

WORKDIR /usr/src/app
ENV NODE_OPTIONS=--openssl-legacy-provider

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build


FROM node:18-alpine

WORKDIR /app

RUN npm install -g serve

COPY --from=build /usr/src/app/build ./build

EXPOSE 3000

CMD ["serve", "-s", "build", "-l", "3000"]
