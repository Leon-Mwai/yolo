# Stage 1: Build React app
FROM node:18-alpine AS build

WORKDIR /usr/src/app
ENV NODE_OPTIONS=--openssl-legacy-provider

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Stage 2: Serve the built app using 'serve'
FROM node:18-alpine

WORKDIR /app

# Install 'serve' globally
RUN npm install -g serve

# Copy the built files from previous stage
COPY --from=build /usr/src/app/build ./build

EXPOSE 3000

# Use 'serve' to host the static files
CMD ["serve", "-s", "build", "-l", "3000"]
