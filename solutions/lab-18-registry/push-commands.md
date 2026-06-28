# Lab 18 — Tag & Push (solution)

```bash
# build the production image locally
docker build -t myapp:1.0 .

# semantic version + the exact commit it was built from
docker tag myapp:1.0 levep79/myapp:1.0
docker tag myapp:1.0 levep79/myapp:$(git rev-parse --short HEAD)

docker login
docker push levep79/myapp:1.0
docker push levep79/myapp:$(git rev-parse --short HEAD)

# verify by pulling back under a clean cache
docker rmi levep79/myapp:1.0
docker pull levep79/myapp:1.0
```

**Why both tags:** `:1.0` is the human-friendly release; the git-SHA tag maps a
running container back to an exact commit. `:latest` is mutable — unfit for
traceability in production.
