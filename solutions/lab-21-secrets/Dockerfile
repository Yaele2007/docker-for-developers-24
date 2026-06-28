# Lab 21 — hardened image + BuildKit secret (no secret in any layer).
# syntax=docker/dockerfile:1
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./

# A token needed only at build time, mounted as a secret — never stored in a layer.
# Build with:  docker build --secret id=token,src=./token.txt -t myapp:secure .
RUN --mount=type=secret,id=token \
    sh -c 'if [ -f /run/secrets/token ]; then export NPM_TOKEN=$(cat /run/secrets/token); fi; npm ci --omit=dev'

COPY --chown=node:node . .
ENV NODE_ENV=production
EXPOSE 3000
USER node
CMD ["node", "server.js"]
