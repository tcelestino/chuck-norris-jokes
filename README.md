# Chuck Norris Jokes

A tribute to actor, martial artist, and American legend Chuck Norris, who passed away on March 20, 2026. This page randomly displays some of the famous jokes the world created in his honor — a way to celebrate the myth he became in pop culture.

## About

A simple web application built with Vue 3 and Vite. On every page load or click on "Next Fact", a new joke is fetched from the public [chucknorris.io](https://api.chucknorris.io) API.

## Stack

- [Vue 3](https://vuejs.org) with Composition API
- [Vite 6](https://vitejs.dev)
- [Chuck Norris API](https://api.chucknorris.io)
- Deployed via [Vercel](https://vercel.com)

## Running locally

```bash
npm install
npm run dev
```

Open `http://localhost:5173`.

## Build

```bash
npm run build
```

The output in `dist/` can be served by any static file server.

## Deploy to Vercel via GitHub Actions

The workflow at `.github/workflows/deploy.yml` automatically deploys on every push to the `main` branch.

Add the following secrets to your repository (Settings > Secrets and variables > Actions):

| Secret | How to obtain |
|---|---|
| `VERCEL_TOKEN` | Vercel > Account Settings > Tokens |
| `VERCEL_ORG_ID` | Run `vercel link` and read `.vercel/project.json` |
| `VERCEL_PROJECT_ID` | Same file as above |
