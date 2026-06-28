# Final Project — Ship the Course App End-to-End

Assemble everything from the week on the course app.

**Deliverables (files in this folder are the reference):**
1. `Dockerfile` — production image: non-root, slim base, prod-only deps, healthcheck.
   (Use a multi-stage build instead if your real app has a build step — see
   `../lab-13-multistage/`.)
2. `compose.yaml` — app + database + healthcheck, ordered startup.
3. Tag + push to your registry — semver **and** git SHA (see `../lab-18-registry/`).
4. A **Trivy** scan with the worst findings addressed (see `../lab-20-scan/`).
5. `ci.yml` — build → test → scan → push (place at `.github/workflows/ci.yml`).

**Done means:**
```bash
docker compose up --build      # whole app comes up clean from scratch
docker push levep79/myapp:1.0  # image in the registry under a meaningful tag
# Trivy shows no fixable CRITICALs; a push to main runs the pipeline green
```

The loop you'll reuse at work: **build → compose → ship → scan → automate.**
