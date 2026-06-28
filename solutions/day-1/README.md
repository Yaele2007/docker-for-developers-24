# Day 1 — Answer Key (Labs 1–10)

Day 1 is CLI-only, so there are no solution *files* — the "answer" is the expected
output and the resolved Stop & think. **Steps live in the lab guide**
(`docs/lab-guide.md`); this is only the outcomes and answers, so try each lab first.

> Instructors: release this on the same gated schedule as the rest of `solutions/`
> — the Stop & think answers are most of the learning.

---

## Lab 1 — Install Check
- `docker version` prints **two** sections: **Client** and **Server**.
- **Stop & think:** the CLI (client) only sends requests; the **daemon** (server) does
  the work. Typing `docker …` = client talking to daemon over the API. That's why two
  versions appear and why a remote daemon is possible.

## Lab 2 — Explore a Container
- `cat /etc/os-release` → Alpine; `whoami` → **root**; `ps aux` → very few processes.
- **Stop & think:** **PID 1** is the shell you started (`sh`), not the host's init.
  Your laptop shows hundreds of processes; the container sees only its own — that's the
  PID namespace.
- Cleanup: `docker rm -f box`.

## Lab 3 — Pull & Pin
- `docker inspect -f '{{index .RepoDigests 0}}' nginx:1.27` prints `nginx@sha256:…`.
- **Stop & think:** `nginx:1.27` is a **tag** — mutable; the maintainers can re-point it
  at a new build tomorrow. The **digest** you recorded is content-addressed and always
  resolves to the exact same image.

## Lab 4 — Container Lifecycle
- After `stop`, the container shows only under `docker ps -a` (status `Exited`).
- **Stop & think:** `stop`→`start` is the **same** instance (its writable layer
  survives). `rm`→`run` is a **new** instance — anything written to the old writable
  layer is gone. Published ports/env are whatever you pass on `run`.

## Lab 5 — Debug a Broken Container
- `docker ps -a` shows **Exit Code 1**; `docker logs broken` →
  `FATAL: DB_HOST environment variable is required`.
- Fix: `docker run -d --name broken -e DB_HOST=db levep79/broken-demo` → stays up.
- **Stop & think:** a non-zero exit code says "failed" before you even read the logs;
  the log line then names the exact missing input.

## Lab 6 — Publish & Configure
- The page/logs reflect `APP_ENV`; `docker port app` shows `3000/tcp -> 0.0.0.0:8080`.
- **Stop & think:** with both `-e` and `--env-file` setting `APP_ENV`, the **`-e` flag
  wins** (later/explicit flag overrides the file). Confirm with
  `docker inspect -f '{{json .Config.Env}}' app` rather than guessing.

## Lab 7 — Tame a Container
- After `docker kill web`, the restart policy brings it back (`docker ps` shows it
  running again, restart count incremented).
- **Stop & think:** exceeding `--memory=256m` triggers an **OOM kill** — the container
  exits with code **137** (128+9, SIGKILL); `docker ps -a` shows it stopped and
  `docker inspect -f '{{.State.OOMKilled}}' web` → `true`.
- Cleanup: `docker rm -f web`.

## Lab 8 — Investigate a Running Container
- `docker diff web` lists `A /tmp/note.txt` (A=added, C=changed, D=deleted).
- **Stop & think:** those changes live in the container's **thin writable layer** on
  top of the read-only image layers. Remove the container and that layer — and the file
  — is gone. (This is the motivation for volumes on Day 2.)
- Cleanup: `docker rm -f web`.

## Lab 9 — Reclaim Disk
- `docker system df` shows reclaimable space; `container prune` + `image prune` shrink it.
- **Stop & think:** `docker system prune -a` removes images **not used by any container**
  — including ones you pulled on purpose. Offline with no wifi, you can't re-pull them.

## Lab 10 — Capstone: Operate a Service (CLI only)
- You run, configure (`--env-file`), limit (`--memory`/`--cpus`), observe (`logs`,
  `exec`, `cp`), inspect, and tear down — all from the CLI.
- **Stop & think:** the repetitive part is re-running a long `docker run` after every
  config change (you had to `rm -f` and retype it). Making that **reproducible** is
  exactly what Day 3's Compose file does.
EOF