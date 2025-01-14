# release-action

 [Create a release](https://docs.github.com/en/rest/releases/releases#create-a-release) through GitHub Action.

## Features
Skip when the tagName's release has already created.

## Inputs

[action.yml](./action.yml).

## Example Usage

*.github/workflows/release-ci.yml*:
```yml
name: Create a release

on:
  push:
    tags:
      - v*

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: lvjiaxuan/release-action@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> [`secrets.GITHUB_TOKEN` is created automatically](https://docs.github.com/cn/actions/security-guides/automatic-token-authentication)

## [Node.js packages publish](https://docs.github.com/cn/actions/publishing-packages/publishing-nodejs-packages)

### 1. Specify `registry-url` of *actions/setup-node*'s inputs for generating *.npmrc* file.

```yml
- uses: actions/setup-node@v2
  with:
    node-version: 16 # ${{ matrix.node }}
    # .npmrc file for authentication would be generated
    # https://npm.pkg.github.com(env.NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }})
    registry-url: https://registry.npmjs.org 
    cache: npm
```

> [Refer](https://github.com/actions/setup-node/issues/82#issuecomment-970324194): *.npmrc* file has more priority than `publshConfig` of package.json, so it will override properties.

### 2. Specify NPM Token for `npm publish`

1. Create your own npm token first(refer to https://www.npmjs.com/settings/{yourname}/tokens)
1. Set it to GitHub Repo: [repo] -> settings -> secrets

```yml
- run: npm publish
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```
