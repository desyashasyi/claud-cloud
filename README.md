# TechUPI — Landing Page

`index.html` is the TechUPI landing page, introducing four platforms:

| Platform | Purpose | Domain |
|---|---|---|
| ArSys | Final project & thesis management | https://arsys.techupi.id |
| Programee | Programming classes | https://programee.techupi.id |
| CLab | Remote IoT laboratory | https://clab.techupi.id |
| ArsipM | Archive & accreditation readiness | https://archim.techupi.id |

App screenshots live in `screenshots/`; click any screenshot on the page to enlarge it.
The page loads Tailwind CSS (CDN) and Font Awesome, so it needs an internet connection.
Deploy `index.html` together with the `screenshots/` folder.

## Deploy to techupi.id

`.github/workflows/deploy.yml` uploads `index.html` and `screenshots/` to the VPS over SSH on every push that changes them (or manually from the Actions tab). Before each upload it backs up the current files to `~/techupi-backups/<timestamp>/` on the server. Other files in the web root are left untouched.

Add these repository secrets (Settings → Secrets and variables → Actions):

| Secret | Value |
|---|---|
| `VPS_HOST` | Server IP or hostname |
| `VPS_USER` | SSH user that can write to the web root |
| `VPS_PATH` | Web root of techupi.id, e.g. `/var/www/techupi.id` |
| `VPS_SSH_KEY` | Private key of a deploy key whose public key is in the user's `~/.ssh/authorized_keys` |
| `VPS_PORT` | Optional, defaults to 22 |
| `VPS_KNOWN_HOSTS` | Optional, output of `ssh-keyscan <host>`; otherwise the host key is scanned at deploy time |

Until the required secrets exist, the workflow skips the deploy with a warning.
