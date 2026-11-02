# iare-bus-enroute keep-alive

Public, secret-free helper repo for **IARE Bus EnRoute** (https://enroute.iare.ac.in).

Render's free instance sleeps after 15 minutes without inbound HTTP traffic, and
the first request after that waits ~50 seconds. This workflow keeps it awake
from **04:00 to midnight IST** so nobody meets a cold start on the morning
buses, and lets it sleep 00:00-04:00 to stay inside the 750-hour free allowance.

## How it works

- Each run pings `/health` every 5 minutes for **5.5 hours**, then dispatches its
  own successor. The chain is continuous, so it does not depend on GitHub's
  scheduler being punctual.
- A `*/30` cron is a **watchdog**: if the chain ever dies, it restarts it.
- A guard step makes duplicate segments exit immediately.
- `/health` does not touch the database, so these pings cost no Neon compute.

## Why a separate public repo

The application repository is private, where a 5-minute cron would consume
~8,600 of the 2,000 free Actions minutes per month and also block CI and
deploys. Public repositories have unlimited Actions minutes. This repo contains
no application code, no credentials, and no student data - only the public
health URL.

## Operating it

- Pause: disable the workflow in the Actions tab (Render then sleeps when idle).
- Change the URL: set the repository variable `APP_HEALTH_URL`.
- Stop for good: delete this repository.
