# Page Studio — API

Backend for Page Studio, a CMS-driven landing-page builder. Editors change drafts; publishers ship **immutable, semantically versioned releases** — each publish diffs the draft against the last release, picks the version bump automatically and writes a changelog.

**Stack:** Node.js · Express · TypeScript · MongoDB (Mongoose) · JWT (access + refresh) · Zod · Helmet · GitHub Actions · Vercel

Frontend: [page-studio-frontend](https://github.com/Arpitpatidar00/page-studio-frontend)

## Publishing model

`src/domain/publish/` is pure, framework-free logic:

- **diff** — structural diff between the draft and the latest release
- **semver** — bump rules: removed sections, changed section types or required props → **major**; added sections or optional props → **minor**; everything else → **patch**
- **changelog** — human-readable entries generated from the diff
- **hash** — content hash so identical drafts never create a new release

## API

| Method | Route | Role | Purpose |
| :-- | :-- | :-- | :-- |
| `POST` | `/api/auth/register`, `/login`, `/refresh` | — | Auth with access + refresh tokens |
| `GET` | `/api/auth/me` | signed in | Current user |
| `GET` | `/api/pages`, `/api/pages/:slug` | — | Page slugs and drafts |
| `PUT` | `/api/pages/:slug` | editor, publisher | Update a draft |
| `GET` | `/api/publish/preview` | — | Preview diff, bump and changelog |
| `POST` | `/api/publish` | publisher | Create a release |
| `GET` | `/api/publish/releases` | — | Release history |
| `GET` `POST` `PATCH` `DELETE` | `/api/admin/users` | `x-admin-key` | Manage users and roles |

Roles: `viewer` · `editor` · `publisher`.

## Run locally

```bash
npm install
npm run dev      # ts-node + nodemon
npm run build && npm start
```

CI builds every pull request to `main`.
