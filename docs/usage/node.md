---
title: Node.js Versions
description: Node.js versions support in Renovate
---

# Node.js Versions

Renovate can upgrade the [Node.js](https://nodejs.org/en/) runtime used by your project.
This way you're using the latest bug fixes, performance improvements, security mitigations, etc.

## File Support

Renovate can manage the Node.js version in the following files:

- The [`engines`](https://docs.npmjs.com/files/package.json#engines) field in [`package.json`](https://docs.npmjs.com/files/package.json)
- The [`volta`](https://docs.volta.sh/guide/understanding#managing-your-project) field in [`package.json`](https://docs.npmjs.com/files/package.json)
- The [`.nvmrc`](https://github.com/creationix/nvm#nvmrc) file for the [Node Version Manager](https://github.com/creationix/nvm)
- The [`node_js`](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/#Specifying-Node.js-versions) field in [`.travis.yml`](https://docs.travis-ci.com/user/customizing-the-build/)

## How It Works

Node.js renovation in `package.json > engines` and in `.nvmrc` is enabled by default, if your existing version is pinned.

To enable `.travis.yml` renovation, you must:

1. Enable Travis renovation explicitly by setting the following Renovate configuration: `"travis": { "enabled": true }`
1. Optionally, configure a support policy (As documented below)

When Renovate processes your project's repository it will look for the files listed above and submit a single pull request that upgrades all Node.js versions simultaneously.

## Configuring Support Policy

Renovate supports a [`supportPolicy`](/configuration-options/#supportpolicy) option that can be passed the following values and associated versions (current as of Nov 2020):

**Default:** `lts`

| supportPolicy | versions       | description                                              |
| ------------- | -------------- | -------------------------------------------------------- |
| all           | 10, 12, 14, 15 | All releases that have not passed their end date         |
| lts           | 10, 12, 14     | All releases classified as LTS, including in maintenance |
| active        | 12, 14, 15     | All releases not in maintenance                          |
| lts_active    | 12, 14         | All releases both LTS and active                         |
| lts_latest    | 14             | The latest LTS release                                   |
| current       | 15             | The latest release from "all"                            |

The version numbers associated with each support policy will be updated as new versions of Node.js are released, moved to LTS or maintenance, etc.

For example, to instruct Renovate to upgrade your project to the latest [Long-term Support](https://github.com/nodejs/Release#release-plan) release, you can use the following configuration:

```json
"supportPolicy": ["lts_latest"]
```

We recommend that you define this support policy inside the `node` configuration object.
This way, it is applied to all Node.js-related files.

For additional language support see the [`supportPolicy` documentation](/configuration-options/#supportpolicy).
