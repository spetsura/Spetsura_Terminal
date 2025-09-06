---
title: "Notes"
---

## [0] VPS provisioning on Oracle Cloud (Free Tier instance)

- Registered an Oracle Cloud account and activated the Always Free Tier.
- Created a VM instance (Ubuntu 22.04), picking a region with available resources.
- Set up SSH access and updated the system (`apt update && apt upgrade`).
- Installed UFW, opened ports 22 (SSH), 80 and 443 (HTTP/HTTPS).

## [1] Domain registration, DNS setup, Cloudflare integration, HTTPS

- Bought the domain **spetsura.com** via Dynadot.
- Connected it to Cloudflare (free tier) and configured DNS (A-record pointing to the VPS).
- Enabled proxying and generated an Origin SSL certificate via Cloudflare.
- Deployed the certificate to `/etc/ssl/cloudflare/` and configured Nginx for HTTPS.

## [2] Ubuntu setup: Nginx, systemd, firewall config

- Installed Nginx via apt, configured site block in `/etc/nginx/sites-available/default`.
- Added HTTP → HTTPS redirect, enabled gzip compression and basic caching headers.
- Enabled systemd service and ensured Nginx autostarts on boot.
- Configured UFW rules with `ufw allow 'Nginx Full'` and basic security settings.

## [3] Manual HTML/CSS structure, no frameworks, responsive layout

- Built the entire HTML/CSS layout from scratch without using templates or frameworks.
- Designed the site in a terminal-style aesthetic, using only plain HTML and CSS.
- Implemented responsive design with media queries.
- Created a modular `style.v5.css` and linked it using cache-busting (`?v=9999`).

## [4] Scroll-triggered animations with IntersectionObserver

- Used the `IntersectionObserver` API to animate sections on scroll.
- Elements with `.hidden` class are revealed by toggling `.visible` when they enter the viewport.
- Applied to sections like `#about-me`, `#work-experience`, and `#certifications`.
- All handled with plain JavaScript — no libraries involved.

## [5] Dynamic adaptation for mobile devices

- Refactored mobile layout to support small screen sizes (tested on iPhone 12 Pro).
- Fixed scroll lock issue caused by intro animation + menu visibility.
- Introduced burger menu with animated transitions and proper open/close logic.
- Hid "About me" section on first screen — now appears on scroll with `slide-up` and `fade-in` animations.
- Verified adaptive behavior of interactive tabs and terminal UI on narrow viewports.

## [6] Added a `/resume` page

- Created a new route `/resume` as a standalone page.
- Designed in light theme by default, with support for toggling to "terminal" (dark) mode.
- Populated it with structured professional resume and a **Download PDF** button.

## [7] Security basics

- Disabled root login over SSH: `PermitRootLogin no`.
- Disabled password authentication: `PasswordAuthentication no`.
- All SSH access now strictly over private key. SSH remains on port 22.
- No public login forms or admin panels exposed — no attack surface for bruteforce.
- Decided **not** to install Fail2Ban — no password auth ⇒ nothing to protect.
- Firewall layer already enforced via Cloudflare proxy.
- Hardened Nginx config with essential security headers:
  - `X-Content-Type-Options: nosniff`
  - `X-Frame-Options: DENY`
  - `Referrer-Policy: no-referrer`
  - `Content-Security-Policy: default-src 'self'`
  - Ensured no conflict with Cloudflare headers.

## [8] Server metrics via Netdata

- Deployed real-time server monitoring using Netdata on a dedicated subdomain: `netdata.spetsura.com`.
- Tracks core metrics: CPU, RAM, disk I/O, network traffic, system load, and active processes.
- Integrated Telegram alerts for critical conditions — instant notifications if something goes wrong.
- Traffic is proxied through Cloudflare to hide the server's origin IP.
- Direct access is restricted by IP allowlist — only trusted IPs can reach the interface.

## [9] Editing and deploying, GitHub integration

- At first, edited files directly on the server via terminal (manual HTML/CSS/JS).
- As configs and codebase grew, switched to WebStorm; worked locally and uploaded changes over SFTP/SSH (convenient but manual, with risks).
- Eventually set up a proper CI/CD pipeline with GitHub Actions and a self-hosted runner:
  - Build → package artifact → deploy into timestamped `releases/` → atomic `current` symlink.
  - Manual approval before prod.
  - Keep last few releases to enable quick rollback.

## [10] Everyday Backups (restic via SFTP)

- Goal: be able to roll back in minutes even if deploy goes wrong or GitHub/CI is down.
- Stack: local working copy (WebStorm) → GitHub repo (sources) → 3 rotating prod releases → off-site **restic** repo over SFTP (20 encrypted snapshots).
- Schedule: nightly at `00:00`, incremental with dedup + encryption; SSH access is key-only; retention `keep-last 20`.
- What I back up: `/var/www/<site>/releases`, `/etc/nginx`, `/etc/letsencrypt`, `/etc/systemd/system`, deploy SSH keys and `.env`.
- What I skip: sources and `node_modules` (sources live in GitHub; deps are rebuildable).
- Quick restore drill:
  - `restic restore latest --target /tmp/restore --include /etc/nginx/nginx.conf`
  - `diff -q /etc/nginx/nginx.conf /tmp/restore/etc/nginx/nginx.conf` → *OK*.
- **Bottom line:** four safety nets in place: Local → GitHub → 3 prod releases → off-site restic snapshots ×20.

## [11] Lessons learned and potential improvements

- Built and deployed a website from scratch without frameworks.
- Improved CSS/layout skills, especially around responsiveness.
- Worked with Cloudflare, SSL, and domain-level configurations.
- Set up CI/CD with GitHub Actions to automate deployments, reduce manual steps, and enable rollbacks.
- **Next steps:**
  - Add a sitemap and better SEO structure.
  - Eventually build a mini admin interface for dynamic content.
