# Publishing `@wppconnect/zapo`

Publishes the **root** package of this monorepo to npm as **`@wppconnect/zapo`**
from the **`publish`** branch via **OIDC trusted publishing** (no npm token).

> zapo is a monorepo (`workspaces: packages/*`). Only the root client package is
> published, with `--workspaces=false`. The sub-packages (store-*, media-utils,
> ...) are not republished here — decide on those separately if needed.

## Why a `publish` branch

- `master` mirrors upstream (vinikjkkj/zapo) → syncing/rebasing is conflict-free.
- `publish` is the only branch where `package.json` has the scoped `name` and
  the fork's `repository.url` (OIDC provenance requires the repo to match).

## First publish (manual — once, needs npm access)

```bash
git fetch origin
git checkout publish
npm ci || npm install
npm run build

npm login                 # account with rights on the @wppconnect scope
npm publish --workspaces=false --access public --provenance
```

> Requires npm ≥ 11.5.1 and Node ≥ 22.14.0.

## Configure the Trusted Publisher

On npmjs.com → `@wppconnect/zapo` → **Settings → Trusted Publishers → Add** →
GitHub Actions:

- Repository: `wppconnect-team/zapo`
- Workflow file: `.github/workflows/publish-wppconnect.yml`

Then every push to `publish` publishes the root package automatically via OIDC.

## Releasing a new version

```bash
git checkout publish
git rebase master
npm version <new-version> --no-git-tag-version
git commit -am "release: @wppconnect/zapo vX.Y.Z"
git push origin publish
```
