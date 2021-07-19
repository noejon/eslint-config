Create repository in github

git clone your repo

yarn init
package name: @USERNAME/REPONAME

mkdir src

touch index.js

yarn to generate yarn.lock

mkdir .github/worflows

touch .github/workflows/release-package.yaml

Paste the following code in the release-package file

```yaml
name: Node.js Package

on:
  release:
    types: [created]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 12
      - run: npm ci
      - run: npm test

  publish-gpr:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 12
          registry-url: https://npm.pkg.github.com/
      - run: npm ci
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

package.json add 
```json
"publishConfig": {
    "@noejon:registry": "https://npm.pkg.github.com"
}
```

create src/index.js

This file contains the base eslint rules

```js
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/eslint-recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier/@typescript-eslint',
    'plugin:prettier/recommended',
    'plugin:jest/recommended',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 2020,
    sourceType: module,
  },
  rules: {
    curly: 2,
    'no-duplicate-imports': 2,
    '@typescript-eslint/explicit-function-return-type': 2,
    '@typescript-eslint/explicit-module-boundary-type': 2,
    '@typescript-eslint/no-explicit-any': 2,
    '@typescript-eslint/no-unused-vars': 2,
    'keyword-spacing': 2,
    'prettier/prettier': [
      2,
      {
        bracketSpacing: true,
        semi: true,
        singleQuote: true,
        trailingComma: 'all',
        arrowParens: 'avoid',
      },
    ],
  },
}
```