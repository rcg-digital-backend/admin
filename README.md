# admin

This repository manages the Safe Settings configs that are applied to an entire organization or arbitrarily defined suborgs.

## Structure Explanation

### `.github/settings.yml`

These are organization-wide settings to apply to everyone in this organization. *We do not use this typically. We govern by `suborgs`.*

https://github.com/github/safe-settings/blob/a427c01bfa6cc1687d57918ab0293d05fa272baf/docs/sample-settings/settings.yml

### `.github/suborgs/*.yml`

Each file here is an arbitrary grouping of repos as defined by `suborgrepos`, each representing a team or namespace.

The entries in `suborgrepos` should be the set of name prefixes common for their team.

https://github.com/github/safe-settings/blob/a427c01bfa6cc1687d57918ab0293d05fa272baf/docs/sample-settings/suborg.yml

The root level properties that can be configured at both the suborg and org levels are always the same. They are documented here: https://github.com/github/safe-settings/tree/a427c01bfa6cc1687d57918ab0293d05fa272baf/docs/github-settings

## References

- **[Platform - Safe Settings - Repo](https://github.com/rcg-digital-backend/platform.safe-settings)**
- **[Safe Settings - Docs](https://github.com/github/safe-settings/tree/a427c01bfa6cc1687d57918ab0293d05fa272baf/docs)**
- **[Safe Settings - Repo](https://github.com/github/safe-settings)**
- **[Safe Settings - Docker Image](https://hub.docker.com/r/yadhav/safe-settings)**
