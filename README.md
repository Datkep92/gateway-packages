# Datkep92/gateway-packages

An APT source for jailbroken iOS, published with Apelin.

Add `https://datkep92.github.io/gateway-packages/` in Irisin.

## How it works

- Packages are the `.deb` files in `debs/`, named
  `<Package>_<Version>_<Architecture>.deb`. For an Apelin-built release,
  use Apelin's Publish flow. For an upstream release published by its own CI,
  download the `.deb` from the upstream's official GitHub Release and verify
  its SHA-256 against that release's `SHA256SUMS` before adding it here.
  Never replace an existing filename with different package bytes.
- On every push to `main`, `.github/workflows/update-repository.yml` runs
  `scripts/update-repository.sh`, which indexes `debs/` into `Packages`,
  `Packages.xz` and `Release`, and publishes the site to GitHub Pages.
  Never commit those indexes or `_site/`.
- `depictions/<package id>/` holds a package's depiction, icon and
  banner; `sileo-featured.json` the banners Irisin shows at the top of the
  source; `CydiaIcon.png`, when present, the source's icon.

Pages may serve the previous index for up to ten minutes after a push.
