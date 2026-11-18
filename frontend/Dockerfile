# syntax=docker/dockerfile:1

# ---------- Stage 1: build the React app with Vite ----------
FROM node:20.18.1-alpine3.20 AS build
WORKDIR /app

COPY package*.json ./
RUN npm install --no-audit --no-fund

COPY . .
RUN npm run build

# ---------- Stage 2: serve the static build with Nginx ----------
FROM nginx:1.27.3-alpine

# Our config serves the React build and proxies /api and /health to the backend
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

HEALTHCHECK --interval=15s --timeout=5s --start-period=10s --retries=5 \
  CMD ["wget", "-q", "--spider", "http://127.0.0.1:80/"]
