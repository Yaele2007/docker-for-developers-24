# Production image for the course app (Lab 14 / Final Project).
# Single stage on purpose: this app has no build/compile step to isolate.
# For an app WITH a build step, see solutions/lab-13-multistage/Dockerfile.
FROM node:20-alpine

WORKDIR /app

# Dependencies first — cached until package*.json changes
COPY package*.json ./
RUN npm ci --omit=dev

# Then the source — code changes don't bust the dependency cache above
COPY --chown=node:node . .

ENV NODE_ENV=production
EXPOSE 3000

# Drop root before the app runs
USER node

# How Docker (and Compose) knows the container is actually healthy
HEALTHCHECK --interval=10s --timeout=3s --start-period=5s \
  CMD node -e "require('http').get('http://localhost:3000/health',r=>process.exit(r.statusCode===200?0:1)).on('error',()=>process.exit(1))"

CMD ["node", "server.js"]
