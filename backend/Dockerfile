# syntax=docker/dockerfile:1

# Pinned, small Alpine-based Node.js image
FROM node:20.18.1-alpine3.20

ENV NODE_ENV=production \
    PORT=3000

WORKDIR /app

# Dependency manifests first -> this layer is cached until they change
COPY package*.json ./
RUN npm install --omit=dev --no-audit --no-fund && npm cache clean --force

# Application source last (changes most often)
COPY --chown=1000:1000 src ./src

# Do not run as root (uid 1000 = the built-in 'node' user)
USER 1000

EXPOSE 3000

HEALTHCHECK --interval=15s --timeout=5s --start-period=40s --retries=5 \
  CMD ["wget", "-qO-", "http://127.0.0.1:3000/health"]

CMD ["node", "src/server.js"]
