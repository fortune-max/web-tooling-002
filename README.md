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

- Use `npx eslint yourfile.js` to lint a specific file. To use eslint in terminal, on commit, use `npx eslint . --ext .js,.jsx,.ts,.tsx` to format all files in current project directory matching those extensions. This can be mapped to run as `npm run lint` command by running `npm pkg set scripts.lint="npx eslint . --ext .js,.jsx,.ts,.tsx"`

#### ESLint-config-prettier

- Allows ESLint to play nicely with Prettier
   
```npm install --save-dev eslint-config-prettier```

- Add `plugin:prettier/recommended` as the last extension in your `.eslintrc.json`. (This compresses a bunch of `Prettier` configs in one plugin eg `"plugins": ["prettier"]` and more).

```
  {
    "extends": ["plugin:prettier/recommended"]
  }
```


#### ESLint-plugin-prettier

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

Extension `plugin:prettier/recomended` expands to:

```
{
  "extends": ["prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error",
    "arrow-body-style": "off",
    "prefer-arrow-callback": "off"
  }
}
```

#### Additional ESLint config for React testing

If you used `create-react-app` or similar, there should be an `eslintConfig.extends` in your `package.json`. Move these two extensions (usually `"react-app"` and `"react-app/jest"`) to your `extends` in `.eslintrc.json` (must not be after last `extends` entry - `plugin:prettier/recommended`).

## Husky Install (Add lint-staged to run your linter on only staged files [optional])

- Install `husky` and `lint-staged` dev dependency

```
npm install --save-dev husky lint-staged
npx husky install
npm pkg set scripts.prepare="husky install"
npm run prepare
```

Then add a Husky pre-commit hook:

```npx husky add .husky/pre-commit "npx lint-staged"``` to run `lint-staged` on every commit event or ```npx husky add .husky/pre-commit "npm test"``` to run whatever tests have been set to run in `package.json`.

You can have more than one pre-commit commands. They are found and can also be viewed and edited in the `.husky/pre-commit` file.

To add the linting npm command we added earlier (`npm run lint`) to be run before every commit we add it to husky with `npx husky add .husky/pre-commit "npm run lint"`.

- Configure lint-staged by adding this to ```package.json```

```
  "lint-staged": {
    "**/*": "prettier --write --ignore-unknown"
  }
```

