**User service**
# 1. Uses an appropriate Node.js base image
FROM node:20-alpine

# 2. Sets a working directory
WORKDIR /usr/src/app

# 3. Installs dependencies
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# 4. Exposes the correct port
EXPOSE 3000

# 5. Includes a valid CMD instruction
CMD ["node", "app.js"]

**Product service**
# 1. Uses an appropriate Node.js base image
FROM node:20-alpine

# 2. Sets a working directory
WORKDIR /usr/src/app

# 3. Installs dependencies
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# 4. Exposes the correct port
EXPOSE 3001

# 5. Includes a valid CMD instruction
CMD ["node", "app.js"]

**Order service**
# 1. Uses an appropriate Node.js base image
FROM node:20-alpine

# 2. Sets a working directory
WORKDIR /usr/src/app

# 3. Installs dependencies
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# 4. Exposes the correct port
EXPOSE 3002

# 5. Includes a valid CMD instruction
CMD ["node", "app.js"]

**Gateway service**
# 1. Uses an appropriate Node.js base image
FROM node:20-alpine

# 2. Sets a working directory
WORKDIR /usr/src/app

# 3. Installs dependencies
COPY package*.json ./
RUN npm install

# Copy application source code
COPY . .

# 4. Exposes the correct port
EXPOSE 3003

# 5. Includes a valid CMD instruction
CMD ["node", "app.js"]
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

**Docker compose file**
version: '3.8'

services:
  user-service:
    build: ./user-service
    container_name: user-service-app
    ports:
      - "3000:3000"
    networks:
      - app-network

  product-service:
    build: ./product-service
    container_name: product-service-app
    ports:
      - "3001:3001"
    networks:
      - app-network

  gateway-service:
    build: ./gateway-service
    container_name: gateway-service-app
    ports:
      - "3003:3003"
    networks:
      - app-network
    depends_on:
      - user-service
      - product-service
  
  order-service:
    build: ./order-service
    container_name: order-service-app
    ports:
      - "3002:3002"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

