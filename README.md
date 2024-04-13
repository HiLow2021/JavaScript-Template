# JavaScript Template

## 概要

- JavaScript のテンプレートリポジトリ。
- ESLint (コード静的解析)、Prettier (コード整形)、husky (git hook によるコード品質担保)、lint-staged (変更ファイルのみスクリプト適用) 導入済み。

## リポジトリを初めから作成する場合

### Node.js プロジェクト初期化

1. npm init -y
    - -y は node.js プロジェクト初期化時の選択をスキップ。

### ESLint (コード静的解析) 導入

1. npm init @eslint/config
    - ESLint は、コードのエラーチェックを行える linter と呼ばれるパッケージ。
    - 詳細: https://eslint.org/docs/user-guide/configuring/

2. .eslintrc に ESLint のルールを設定できる。このリポジトリの .eslintrc は以下の通り。

```json
{
    "env": {
        "es2021": true,
        "node": true
    },
    "extends": ["eslint:recommended", "prettier"],
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "rules": {
    }
}
```

### Prettier (コード整形) 導入

1. npm i --save-dev prettier eslint-config-prettier
    - Prettier は、コードを整形してくれるパッケージ。
    - eslint-config-prettier は、eslint にもコード整形の機能があるので、双方の調整をするために必要。
    - 詳細: https://prettier.io/docs/en/index.html

2. .eslintrc の extends の最後に 'prettier' を加える。
    - 今までは、'prettier', 'prettier/@typescript-eslint' の二つを記述する必要があったが、"prettier/@typescript-eslint" は eslint-config-prettier の "prettier" にマージされた。

3. .prettierrc に prettier の整形ルールを設定できる。このリポジトリの .prettierrc は以下の通り。

```json
{
    "printWidth": 140,
    "tabWidth": 4,
    "useTabs": false,
    "singleQuote": true,
    "trailingComma": "none",
    "semi": true,
    "endOfLine": "crlf"
}
```

### husky (Git hook パッケージ) 導入

1. npm install --save-dev husky
    - husky は、Git hook による Git コマンド前後にスクリプトを自動実行するパッケージ。
    - Git Commit 前に ESLint を実行して、エラーが発生する場合はコマンドをキャンセルすることにより、コードの品質を担保することができる。
    - 詳細: https://typicode.github.io/husky

2. npx husky init
    - <span style="color: orange; ">事前に git init で Git を初期化しておくこと。</span>

3. .husky/* ファイル (eg. ./husky/pre-commit) に実行したい npm スクリプトを記述。

    例 (.husky/pre-commit)
```sh
npm run format
npm run lint
```

### lint-staged (Staging ファイルのみスクリプト適用) 導入

1. npm install --save-dev lint-staged
    - lint-staged は Staging ファイルに限定して、スクリプトを適用するパッケージ。
    - コード解析やコード整形の処理時間を短縮することができる。

2. .lintstagedrc を作成して lint-staged 対象ファイルとスクリプトを記述する。このリポジトリの .lintstagedrc は以下の通り。

```json
{
    "./**/*.{js,jsx}": "npx eslint",
    "./**/*.{js,jsx,css,scss}": "npx prettier --write"
}
```

3. .husky/* ファイル (eg. ./husky/pre-commit) に lint-staged を記述。

    例 (.husky/pre-commit)
```diff
- npm run format
- npm run lint
+ npx lint-staged
```

### npm scripts 例

```json
"scripts": {
    "start": "node ./index.js",
    "lint": "npx eslint ./**/*.{js,jsx}",
    "lint-staged": "npx lint-staged",
    "format": "npx prettier --write ./**/*.{js,jsx,css,scss}",
    "prepare": "husky"
},
```