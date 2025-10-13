# === Stage 1: Build ===
FROM node:18-alpine AS build

WORKDIR /usr/src/app

# Copy package files and install deps
COPY package*.json ./
RUN npm install --production

# Copy rest of the code
COPY . .

# === Stage 2: Run ===
FROM alpine:3.16

WORKDIR /app

# Install Node.js runtime
RUN apk add --no-cache nodejs npm

# Copy app from build stage
COPY --from=build /usr/src/app /app

# Expose backend port
EXPOSE 5000

# Start the server
CMD ["node", "server.js"]
