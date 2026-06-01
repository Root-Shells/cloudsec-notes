# Cloud Security Notes

Quartz-powered GitHub Pages project site for cloud security engineering notes.

Live site, once Pages is enabled and the first workflow completes:

https://root-shells.github.io/quartz-blog/

## Local Development

```bash
npm install
npx quartz build --serve
```

Write notes in `content/`. The GitHub Actions workflow publishes the site from the `v5` branch.

## Deploy

Push changes to GitHub, then set repository Pages source to **GitHub Actions** in the GitHub repository settings.
