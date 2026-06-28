# Lab 13 — multi-stage build (TECHNIQUE DEMO).
#
# NOTE: the course app has no build/compile step, so it does not actually
# need multi-stage — its real production image is single-stage
# (see solutions/lab-14-production/Dockerfile). This file demonstrates the
# pattern you WILL use for any app with a build step (TypeScript, a bundler,
# Maven, etc.): build in a fat stage, copy only the artifact into a slim one.

# ---- build stage: full toolchain ----
FROM node:20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build            # produces dist/  (illustrative)

# ---- run stage: slim, only the artifact ----
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=build --chown=node:node /app/dist ./dist
USER node
EXPOSE 3000
CMD ["node", "dist/server.js"]
