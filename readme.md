# spetsura.com — my personal terminal-style resume website

This is my personal portfolio site, styled as a terminal interface. It reflects my background, mindset, and attitude toward technology. Everything is handcrafted: from HTML/CSS to animations and layout logic.

## What's inside

- Terminal-like intro animation
- `whoami` section — personal summary as a CLI command
- Work experience with tab navigation
- Responsive design for mobile (burger menu included)
- Light/dark theme support (/resume page)
- Downloadable PDF resume via `/resume`
- Dynamic content editing via Decap CMS (Markdown + Git backend)
- Automated CI/CD pipeline with GitHub Actions (self-hosted runner, release rotation, nginx reload)
- Basic SEO setup (sitemap.xml, robots.txt, Open Graph for social previews)

## Built with

- Pure HTML, CSS, vanilla JS
- Decap CMS for content editing
- GitHub for version control & CI/CD (self-hosted runner)
- Oracle Cloud VPS + Nginx + systemd
- Cloudflare for DNS, SSL, and security (proxy, firewall)
- Restic for automated off-site backups

## Why I built this

To demonstrate not just "tech experience", but how I work:

- Clean, minimal, framework-free architecture
- From manual deploys → to fully automated GitHub Actions pipeline
- Full control over logic, layout, hosting, backups, and monitoring
- Realistic DevOps practice on a personal project

## Live version

[spetsura.com](https://spetsura.com)

## License

MIT — feel free to fork, learn, and remix.
