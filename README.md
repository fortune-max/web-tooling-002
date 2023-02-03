# web-tooling-002

## Prettier Setup

- Install Prettier

```npm install --save-dev --save-exact prettier```

- Alert VSCode Prettier is used on project

```echo {}> .prettierrc.json```

- Add files for Prettier to ignore

```echo "# Ignore artifacts:\nbuild\ncoverage" > .prettierignore```

- Use ```npx prettier --check .``` or ```npx prettier --write .``` to check if project is formatted (dry-run) or format the project.


- Replace ```.``` with wildcards or specific files eg.

```
prettier --write app/
prettier --write app/components/Button.js
prettier --write "app/**/*.test.js"
```

## ESLint Setup

-  Install and configure ESLint for your project through the terminal prompt depending on your project type by running:

```npm init @eslint/config```

- Use ```npx eslint yourfile.js``` to lint a specific file. (This may not be necessary as this is usually auto-managed)

## ESLint-config-prettier

- Allows ESLint to play nicely with Prettier
   
```npm install --save-dev eslint-config-prettier```

- Add `plugin:prettier/recommended` as the last extension in your `.eslintrc.json`. (This compresses a bunch of `Prettier` configs in one plugin eg `"plugins": ["prettier"]` and more).

```
  {
    "extends": ["plugin:prettier/recommended"]
  }
```


## ESLint-plugin-prettier

This runs `Prettier` as an ESLint rule and works with `eslint-config-prettier` to prevent conflicts

- First install `eslint-plugin-prettier`

`npm install --save-dev eslint-plugin-prettier`

Then add to `.eslintrc.json`:

```
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
```

(You can ignore the above config if you used the `extend` config with `"plugin:prettier/recommended"` in `eslint-config-prettier` from earlier).

## Husky Install (And lint-staged to run your linter on only staged files [optional])

- Install `husky` and `lint-staged` dev dependency

```
npm install --save-dev husky lint-staged
npx husky install
npm pkg set scripts.prepare="husky install"
npm run prepare
```

Then add a Husky pre-commit hook:
```npx husky add .husky/pre-commit "npx lint-staged"``` to run `lint-staged` on every commit event or ```npx husky add .husky/pre-commit "npm test"``` to run whatever tests have been set to run in `package.json`

- Configure lint-staged by adding this to ```package.json```

```
  "lint-staged": {
    "**/*": "prettier --write --ignore-unknown"
  }
```

