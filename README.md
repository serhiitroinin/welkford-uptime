# welkford-uptime

Scheduled uptime check for Welkford (https://welkford.com and
https://welkford.co.uk). This repo holds only a GitHub Actions workflow: no
app code, no secrets, no tokens. It lives in a public repo because Actions
minutes are free here; the same check in the private app repo used up its
monthly allowance.

## What it does

`.github/workflows/uptime.yml` runs every 15 minutes (and on demand from the
Actions tab):

1. `GET https://welkford.com/api/health` must answer 200 with `"ok": true`.
   Warnings show as yellow annotations. Only reason codes are logged, never
   the response body. Any other answer, 404 included, fails the run.
2. These public pages must answer and contain their expected words:

   | URL | Expected text |
   |---|---|
   | https://welkford.com/ | `Paste your registrations` |
   | https://welkford.com/guides | `TechMan` |
   | https://welkford.com/signin | `Welkford` |
   | https://welkford.co.uk/ | `part of Welkford` |

A failed run is the alert: GitHub emails a failed scheduled run to whoever
last changed the `cron` line. Make sure Settings → Notifications → Actions
has email on (optionally "Only notify for failed workflows").

A `keepalive` job runs every Monday at 06:17 UTC and re-enables the workflow,
because GitHub switches off schedules in public repos after 60 days without
activity.

## Health reason codes

The meaning of each health reason code is documented in the private app repo
under `docs/uptime.md`.

## Keep the private-repo schedule disabled

The original copy of this workflow in the private app repo is disabled
(`gh workflow disable uptime.yml -R serhiitroinin/welkford`). Leave it that
way: every run there is billed. Do not re-enable it while this repo runs the
check.
