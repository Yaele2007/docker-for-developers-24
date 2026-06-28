# Lab 22 — Root cause (solution)

**Root cause:** `DB_HOST` was set to `databse` (a typo), but Compose's DNS only
resolves the actual **service name**, which is `db`. So the app's `pg_isready -h databse`
could never resolve the host and looped forever.

**How the method finds it:**
1. `docker compose ps` — `app` is up but never reaches a healthy/idle state.
2. `docker compose logs app` — repeats `waiting for databse ...`; pg_isready reports it
   cannot translate the host name. That single word *is* the clue.
3. The hostname an app uses must equal a service name on the Compose network.

**Fix:** change `DB_HOST: databse` → `DB_HOST: db`, then `docker compose up -d` and
confirm the log prints `CONNECTED`.

**Other ways this same symptom shows up (good discussion):**
- Right host, wrong **credentials** (`PGPASSWORD` / `POSTGRES_PASSWORD` mismatch).
- Right host, but the app starts **before** the db is ready — fix with a healthcheck
  + `depends_on: condition: service_healthy` (see Lab 17).

**Stop & think:** `ps` → `logs` → `inspect` cracked this. They're always the opening
moves because they answer, in order: *is it running? what is it saying? how is it
configured?* — narrowing from symptom to cause before you change anything.
