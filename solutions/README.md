# Solutions

Reference answers for the course. **Try each lab first** — the Stop & think prompts
are where the learning is.

- **Day 1 (Labs 1–10)** is CLI-only → see [`day-1/`](day-1/README.md) for an answer key
  (expected output + resolved Stop & think). No solution files, by design.
- **Days 2–3** produce real artifacts → one folder per lab below.

| Folder | Lab | What's inside |
|--------|-----|---------------|
| `day-1/` | 1–10 | CLI answer key |
| `lab-11-build/` | 11 Build Your First Image | basic `Dockerfile` |
| `lab-12-refine/` | 12 Refine the Image | `Dockerfile` + `.dockerignore` |
| `lab-13-multistage/` | 13 Multi-Stage Build | multi-stage `Dockerfile` (technique demo) |
| `lab-14-production/` | 14 Containerize | production `Dockerfile` (non-root, healthcheck) |
| `lab-17-compose/` | 17 Extend the Stack | `compose.yaml` |
| `lab-18-registry/` | 18 Tag & Push | `push-commands.md` |
| `lab-19-ci/` | 19 CI Pipeline | `.github/workflows/ci.yml` |
| `lab-20-scan/` | 20 Scan & Harden | hardened `Dockerfile` + `trivy-notes.md` |
| `lab-21-secrets/` | 21 Secrets & Least Privilege | `Dockerfile` + `run-flags.md` |
| `lab-22-troubleshoot/` | 22 Observe & Troubleshoot | `FIXED-compose.yaml` + `root-cause.md` |
| `final-project/` | Final | `Dockerfile` + `compose.yaml` + `ci.yml` + `README.md` |

> **Note on the app and multi-stage:** the course app is a tiny zero-dependency Node
> service, so its real production image (`lab-14-production/`) is **single-stage** on
> purpose. `lab-13-multistage/` demonstrates the multi-stage *technique* you'd use for
> an app that has a build step. Both are correct — for different apps.

**Instructor tip:** if you want to withhold solutions until after each day, keep this
folder in a separate access-controlled repo (or a private branch) and grant access on
your own schedule. See the repo `README.md`.
