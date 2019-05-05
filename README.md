### 为 TypeScript 设置 ESLint

首先，安装需要的 dev dependencies:

```shell
npm i @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
```

- `eslint` 安装 ESLint 库
- `@typescript-eslint/parser` 解析器，允许 ESLint 解析 ts 代码
- `@typescript-eslint/eslint-plugin` 插件，包含一堆 ESLint 对于 ts 的规则

接下来，添加一个配置文件 `.eslintrc.js` 在项目的根目录。

```js
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  extends: [
    "plugin:@typescript-eslint/recommended" // Uses the recommended rules from the @typescript-eslint/eslint-plugin
  ],
  parserOptions: {
    ecmaVersion: 2018, // Allows for the parsing of modern ECMAScript features
    sourceType: "module" // Allows for the use of imports
  },
  rules: {
    // Place to specify ESLint rules. Can be used to overwrite rules specified from the extended configs
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  }
};
```

**注意：如果你使用 React 搭配 TypeScript，需要添加 `eslint-plugin-react` 到 dev-dependency 并且使用以下的配置**

```js
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  extends: [
    "plugin:react/recommended", // Uses the recommended rules from @eslint-plugin-react
    "plugin:@typescript-eslint/recommended" // Uses the recommended rules from @typescript-eslint/eslint-plugin
  ],
  parserOptions: {
    ecmaVersion: 2018, // Allows for the parsing of modern ECMAScript features
    sourceType: "module", // Allows for the use of imports
    ecmaFeatures: {
      jsx: true // Allows for the parsing of JSX
    }
  },
  rules: {
    // Place to specify ESLint rules. Can be used to overwrite rules specified from the extended configs
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  },
  settings: {
    react: {
      version: "detect" // Tells eslint-plugin-react to automatically detect the version of React to use
    }
  }
};
```

最终还是由你来决定 what rules you would like to extend from and which ones to use within the `rules` object in your `.eslint.rc` file

### 添加 Prettier

```shell
npm i --save-dev prettier eslint-config-prettier eslint-plugin-prettier
```

添加一个 `.prettierrc.js` 文件用来配置 prettier

```js
module.exports = {
  semi: true,
  trailingComma: "all",
  singleQuote: true,
  printWidth: 120,
  tabWidth: 4
};
```

然后，更新 `.eslintrc.js` 文件

```js
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  extends: [
    "plugin:@typescript-eslint/recommended", // Uses the recommended rules from the @typescript-eslint/eslint-plugin
    "prettier/@typescript-eslint", // Uses eslint-config-prettier to disable ESLint rules from @typescript-eslint/eslint-plugin that would conflict with prettier
    "plugin:prettier/recommended" // Enables eslint-plugin-prettier and displays prettier errors as ESLint errors. Make sure this is always the last configuration in the extends array.
  ],
  parserOptions: {
    ecmaVersion: 2018, // Allows for the parsing of modern ECMAScript features
    sourceType: "module" // Allows for the use of imports
  }
};
```

### Auto Fix Code (VS Code)

Setup VS Code to automically run ESLint's automatic fix command (i.e. eslint --fix) whenever a file is saved.

在 VS Code 的设置文件 `settings.json` 文件中添加以下配置：

```json
"eslint.autoFixOnSave":  true,
"eslint.validate":  [
  "javascript",
  "javascriptreact",
  {"language":  "typescript",  "autoFix":  true  },
  {"language":  "typescriptreact",  "autoFix":  true  }
],
```

If you've also set the `editor.formatOnSave` option to true in your `settings.json`, you'll need to add the following config to prevent running 2 formatting commands on save for JavaScript and TypeScript files:

```json
"editor.formatOnSave":  true,
"[javascript]":  {
  "editor.formatOnSave":  false,
},
"[javascriptreact]":  {
  "editor.formatOnSave":  false,
},
"[typescript]":  {
  "editor.formatOnSave":  false,
},
"[typescriptreact]":  {
  "editor.formatOnSave":  false,
},
```
