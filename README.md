# laravel-cpanel-staging-deploy
Automated Laravel staging deployment for Ecenica cPanel using GitHub Actions

## Deploy Laravel to Staging (Ecenica cPanel Users)

The GitHub Action `deploy-staging.yml` is designed for Laravel applications hosted with Ecenica on cPanel.  
It deploys your Laravel app to the staging environment automatically on every push to the `staging` branch.

---

## How to Trigger a Deployment to Staging

1. **Merge your development branch into the `staging` branch**  
   Deployment is triggered automatically on push to the staging branch.

2. **Monitor the Deployment**  
   Go to **GitHub > Actions > Deploy to Staging** in your repository to view build and deployment logs.

---

## Required Secrets in GitHub

The following repository secrets must be set in your GitHub repo settings:

| Secret Name       | Description |
| ----------------- | ----------- |
| `CPANEL_SSH_KEY`  | Private SSH key for your Ecenica hosting account |
| `CPANEL_HOST`     | Your Ecenica server hostname |
| `CPANEL_USERNAME` | Your cPanel SSH username |
| `CPANEL_PATH`     | Path to the Laravel application on the server |

---

## What the Workflow Does

- Installs Node.js and PHP dependencies  
- Builds Laravel front-end assets (e.g. Vite, Mix)  
- Packages the project (respects `.deployignore`)  
- Deploys via `rsync` over SSH to your Ecenica hosting account  
- Writes custom `.htaccess` and `index.php` files to the root for staging  
- Runs Laravel commands:
  - `php artisan config:cache`
  - `php artisan route:cache`
  - `php artisan view:cache`
  - `php artisan migrate --force`

---

## Notes

- Deployment overwrites the target directory (`--delete` flag).  
- Ensure `.deployignore` is set to exclude files/directories not needed in staging.  
- Typical Ecenica staging environment stack:
  - PHP 8.4  
  - LiteSpeed Web Server  
  - MariaDB 10.6  
  - Redis available (optional)  
- Staging branch cloned from main.  
- Main branch must be updated for GitHub Actions to run.
