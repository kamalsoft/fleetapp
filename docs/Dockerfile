# Dockerfile for Node.js microservices (e.g., vehicle-service)

# ---- Base Stage ----
# Use a specific Node.js version for reproducibility.
FROM node:18-alpine AS base
WORKDIR /usr/src/app

# ---- Dependencies Stage ----
FROM base AS dependencies
# Copy package files and install dependencies.
# This layer is cached as long as package files don't change.
COPY package*.json ./
RUN npm install --production

# ---- Build Stage ----
FROM base AS build
COPY package*.json ./
# Install all dependencies, including devDependencies, to run the build.
RUN npm install
COPY . .
# Compile TypeScript to JavaScript.
RUN npm run build

# ---- Production Stage ----
FROM base AS production
COPY --from=dependencies /usr/src/app/node_modules ./node_modules
COPY --from=build /usr/src/app/dist ./dist

CMD [ "node", "dist/main.js" ]