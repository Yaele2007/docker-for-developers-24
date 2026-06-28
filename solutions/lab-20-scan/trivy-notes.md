# Lab 20 — Scan & Harden (solution, Trivy)

You don't have Docker Scout, so use Trivy (runs as a container, nothing to install).

## Scan
```bash
mkdir -p ~/trivy-cache
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  -v ~/trivy-cache:/root/.cache/trivy \
  aquasec/trivy image --severity HIGH,CRITICAL --ignore-unfixed myapp:1.0
```

## Before / after
Build an un-hardened "before" and the hardened "after" (Dockerfile in this folder),
scan both, compare the HIGH/CRITICAL totals.

```dockerfile
# before.Dockerfile — fat base, root user
FROM node:20
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "server.js"]
```

```bash
docker build -f before.Dockerfile -t myapp:before .
docker build -f Dockerfile        -t myapp:after  .
# scan each; the count drops sharply for :after
```

**What moves the numbers:** (1) slim, current base — `node:20-alpine` over `node:20`
removes most OS-package CVEs; (2) `npm ci --omit=dev` drops dev tooling; (3) `USER node`.

**The lesson:** most container risk is inherited from the base image, not your code.
