# react-set-up
reactの環境構築

# 前提
nodeがインストールされていること。
voltaはnodeのバージョン管理に利用
npmはnodeをインストール時に自動的にインストールされるパッケージマネージャー
```bash
$ node -v
v20.13.1
$ volta -v
1.1.1
$ npm -v
10.8.1
```

# Vite
Viteのインストール
```bash
$ npm create vite@latest react-env-setup
Need to install the following packages:
create-vite@5.5.5
Ok to proceed? (y) y


> npx
> create-vite react-env-setup

✔ Select a framework: › React
✔ Select a variant: › TypeScript + SWC

Scaffolding project in /hikaru-champion/react-set-up/react-env-setup...

Done. Now run:

  cd react-env-setup
  npm install
  npm run dev
```

```bash
$ cd react-env-setup
```


```bash
$ npm install
```

開発環境の立ち上げ
```bash
$ npm run dev
```

# path aliasの設定
tsconfig.app.jsonに以下の設定を追加する
```json
    /* import alias*/
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"]
    }
```

tsconfig.app.jsonを変更するとvite.config.tsにも変更を加えなければいけない。
vite-tsconfig-pahtsモジュールをインストールすることで、tsconfig.app.jsonを変更するとvite.config.tsにも変更が入るようになる
```bash
$ npm install -D vite-tsconfig-paths
```

vite
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'
import tsconfigPaths from 'vite-tsconfig-paths'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tsconfigPaths()],
})

```

# Vitest
テストツール
```bash
$ npm install -D vitest happy-dom @vitest/coverage-v8 @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

package.jsonに★の部分を追加する
```json
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "lint": "eslint .",
    "preview": "vite preview",
    "test": "vitest watch", // ★
    "coverage": "vitest run --coverage"// ★
  },
```

vitest-setup.tsをプロジェクトのルートディレクトリに追加する
```ts
import "@testing-library/jest-dom/vitest";
```

vite.config.tsに★の部分を追加する
```ts
// 以下追加
/// <reference types="vitest"/> 
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'
import tsconfigPaths from 'vite-tsconfig-paths'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tsconfigPaths()],
  // 以下追加
  test: {
    globals: true,
    environment: "happy-dom",
    setupFiles: ["./vitest-setup.ts"],
  }
})
```

vite.config.tsに以下の設定を追加する
```ts
  "include": ["src", "vitest-setup.ts"]
```

tsconfig.app.jsonに以下の★の設定を追加する
```json
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"]
    },
    "types": ["vitest/globals"] ★
```

# ESLint
インストール
```bash
$ npm install -D eslint
```

初期設定
```bash
$ npx eslint --init
You can also run this command directly using 'npm init @eslint/config@latest'.
Need to install the following packages:
@eslint/create-config@1.3.1
Ok to proceed? (y) y


> react-env-setup@0.0.0 npx
> create-config

@eslint/create-config: v1.3.1

✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · typescript
✔ Where does your code run? · browser
The config that you've selected requires the following dependencies:

eslint, globals, @eslint/js, typescript-eslint, eslint-plugin-react
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · npm
☕️Installing...

added 99 packages, and audited 357 packages in 3s

135 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Successfully created hikaru-champion/react-set-up/react-env-setup/eslint.config.js file.
```

package.jsonのlintを以下に修正
```
    "lint": "eslint src",
    "lint:fix": "eslint src --fix",
```

以下のコマンドでeslintのチェックを実施
```
$ npm run lint
```

# Prettier
Prettierをインストール
```
npm install -D prettier
```

# husky/lint-staged
git commit時にりんたーを強制実行させるためのツール
以下のコマンドでインストール
```
$ npm install -D husky lint-staged
```

huskyの初期設定を以下のコマンドで実行する。
```
$ npx husky init
```

.husky/pre-commitファイルが自動生成されるので以下を設定する
```
lint-staged
npm test
```

package.jsonに以下の設定を追加する
```json
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "prettier --write",
      "eslint --fix"
    ]
  }
```

# Tailwindcss
[公式ドキュメント](https://tailwindcss.com/)を参照
以下のコマンドでインストール
```
$ npm install -D tailwindcss postcss autoprefixer
```

以下のコマンドで初期化を実施
```
$ npx tailwindcss init -p
```

# shadcn/ui
[公式ドキュメント](https://ui.shadcn.com/docs/installation/vite)を参照
