# Cloud Security Notes

Quartz-powered GitHub Pages site for cloud security engineering notes.

Live site:

https://sec.metoniclabs.com/

## Local Development

```bash
npm install
npx quartz build --serve
```

Write notes in `content/`. The GitHub Actions workflow publishes the site from the `v5` branch.

## Deploy

Push changes to GitHub. The repository is configured to deploy with GitHub Pages using GitHub Actions.
