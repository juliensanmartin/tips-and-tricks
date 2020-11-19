Tips Formatting

# PRETTIER

1. Install nom install -D prettier
2. Install Prettier extension
3. -> settings -> Format On Save
4. -> settings -> Prettier -> Require Config
5. create .prettierrc file with { "singleQuote": true } inside (can go to prettier.io/playground to configure own config for prettier)
6. in package.json create a script: format: "prettier --write \"src/\*_/_.{js,html, jsx, json, yml, yaml, css, less, scss,ts,tsx,graphql}\""
7. add .prettierignore:

```
node_modules
coverage
dist
build
```

# ESLINT

1. npm install -D eslint eslint-config-prettier
2. npm install -D babel-eslint eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react
   // maybe from Kent C Dodds add: npm install -D eslint-config-prettier
   // to remove duplicate rules between eslint and prettier and add that as extends rules in .eslintrc
3. npm i -D eslint-plugin-react-hooks
4. create .eslintrc

```json
{
"extends": [ // order is important, set of rules
"eslint:recommended",
"plugin: import/errors",
"plugin: react/recommended",
"plugin: jsx-a11y/recommended",
"prettier",
"prettier/react"
],
"rules": [ //individual rule
"react/prop-types": 0,
"no-console": 1,
"react/hooks/rule-of-hooks": 2,
"react-hooks/exhaustive-deps": 1
]
"plugins": [ // add ability to eslint
"react",
"import",
"jsx-a11y",
"react-hooks"
],
"parserOptions": {
"ecmaVersion": 2018,
"sourceType": "module", // use module
"ecmaFeatures": {
"jsx": true
}
},
"env": { // add env variable
"es6": true,
"browser": true,
"node": true,
"jest": true
},
"settings": {
"react": {
"version": "detect"
}
}
}
```

5. Add to package.json
   "lint": "eslint \"src/\*_/_ {js,jsx}\" —quiet" //add eslint extension

6. in .gitignore

```
node_modules/
.DS_Store
.cache/
dist/
coverage/
.vscode/
.env
```

extension "npm intellisense" to autocomplete npm package name

```js
// important for React build/dev
NODE_ENV = development;
NODE_ENV = production;
```

// force to not use unstable api etc.

```js
<React.StrictMode>…</React.StrictMode>
```

// for not letting Babel translate unnecessary and let babel and postcss know what browser we want our app to be build with
In package.json

```json
{
  "browserslist": ["last 2 Chrome versions"]
}
```

// From Kent C Dodds
package.json scripts

```json
"test": "jest",
"lint": "eslint src",
"format": "npm run prettier -- --write",
"prettier": "prettier ….",
"validate": "npm run lint && npm run test && npm run — —list-different",
"precommit": "lint-staged && npm run validate" // thanks to husky precommit hook
```

npm install -D husky
// if run —no-verify will bypass precommit hook

npm install -D lint-staged
// run only on updated files
.lintstagedrc

```json
{
  "linters": {
    "*.js": ["eslint"],
    //all extension files for prettier
    "….": ["prettier —write", "git add"]
  }
}
```
