# Lab 21 — Least-privilege run flags (solution)

```bash
# build (BuildKit secret never lands in a layer)
echo "dummy-token" > token.txt
docker build --secret id=token,src=./token.txt -t myapp:secure .
docker history myapp:secure | grep -i token    # nothing leaks

# run: read-only fs + tmpfs, drop all caps, block privilege escalation
docker run -d --name app \
  --read-only --tmpfs /tmp \
  --cap-drop ALL --security-opt=no-new-privileges \
  -p 8080:3000 myapp:secure

curl localhost:8080            # still works
docker exec app whoami         # node, not root
```

## SBOM
```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy image --format cyclonedx -o /dev/stdout myapp:secure > sbom.json
```

**Stop & think:** dropping ALL capabilities and adding none back, yet the app still
runs, means it needed none. Add back only a specific capability if a feature breaks.

**Pitfall:** `--read-only` breaks apps that write to disk. The fix is a `--tmpfs` (or a
volume) on the exact path that needs writing — not removing `--read-only`.
