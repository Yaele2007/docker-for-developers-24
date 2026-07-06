# Lab 12 — refined: dependency caching kept, ENTRYPOINT form.
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
EXPOSE 3000
# ENTRYPOINT fixes the executable; the container behaves like the app itself.
# (This app reads PORT from the environment, so we don't pass a --port arg here.)
ENTRYPOINT ["node", "server.js"]
CMD ["--port=3000"]