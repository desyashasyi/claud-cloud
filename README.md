# TechUPI — Landing Page

`index.html` is the TechUPI landing page, introducing five platforms:

| Platform | Purpose | Domain |
|---|---|---|
| ArSys | Final project & thesis management | https://arsys.techupi.id |
| Programee | Programming classes | https://programee.techupi.id |
| CLab | Remote IoT laboratory | https://clab.techupi.id |
| ArsipM | Archive & accreditation readiness | https://archim.techupi.id |
| FetNet | Course timetabling (FET engine) | https://fetnet.techupi.id |

App screenshots live in `screenshots/`; click any screenshot on the page to enlarge it.
The page loads Tailwind CSS (CDN) and Font Awesome, so it needs an internet connection.
Deploy `index.html` together with the `screenshots/` folder.

## Deploy to techupi.id

`.github/workflows/deploy.yml` uploads `index.html` and `screenshots/` to the VPS over SSH on every push that changes them (or manually from the Actions tab). Before each upload it backs up the current files to `~/techupi-backups/<timestamp>/` on the server. Other files in the web root are left untouched.

To protect the other apps on the server, the deploy stops before writing anything unless:

- an Nginx config in `sites-enabled` or `conf.d` has `server_name techupi.id` with `root` set to `VPS_PATH`;
- `VPS_PATH` holds no application files (`index.php`, `artisan`, `.env`, `package.json`, `composer.json`, `wp-config.php`, `manage.py`);
- `VPS_PATH` is not a broad folder such as `/`, `/var/www`, or `/etc`.

The workflow never edits web server config, restarts services, or deletes files.

Add these repository secrets (Settings → Secrets and variables → Actions):

| Secret | Value |
|---|---|
| `VPS_HOST` | Server IP or hostname |
| `VPS_USER` | SSH user that can write to the web root |
| `VPS_PATH` | Web root of techupi.id, e.g. `/var/www/techupi` |
| `VPS_SSH_KEY` | Private key of a deploy key whose public key is in the user's `~/.ssh/authorized_keys` |
| `VPS_PORT` | Optional, defaults to 22 |
| `VPS_KNOWN_HOSTS` | Optional, output of `ssh-keyscan <host>`; otherwise the host key is scanned at deploy time |

Until the required secrets exist, the workflow skips the deploy with a warning.

## InnoTec research group site

`innotec/` is a Hugo site for https://innotec.techupi.id, with its own templates (no external theme).

- Members: `innotec/data/members.yaml`
- Publications: `innotec/data/publications.yaml`
- Research themes: one Markdown file per project in `innotec/content/projects/`
- News: one Markdown file per post in `innotec/content/news/`

Preview locally with `hugo server` inside `innotec/`. `.github/workflows/deploy-innotec.yml` builds the site and uploads it to `/var/www/innotec` on every push that changes `innotec/`, after the same safety checks as the landing page. Server setup: `deploy/nginx/innotec.techupi.id.conf`.
