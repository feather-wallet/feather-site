# Feather Wallet site

The site is built on top of [Hugo](https://gohugo.io).

## Adding content

- The **Home** page: `layouts/index.html`
- **Download** page: `layouts/section/download.html`
- Changelogs go in `content/changelog`
- **Screenshots** page: `content/screenshot/screenshots.md`
- Release info is located in: `data/releases.json`

## Running the site locally

- Make sure Hugo is installed: `hugo version`
- Clone this repository: `git clone https://github.com/feather-wallet/feather-site.git && cd feather-site`
- Run `hugo server`

Access the site on `http://localhost:1313/`
