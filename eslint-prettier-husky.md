# 🏗️ Angular Lint + Format + Husky Setup (Clean 2026 Version)

# =========================================================

# 🎯 GOAL

# =========================================================

- ESLint → code quality
- Prettier → formatting (standalone)
- Husky + lint-staged → enforce clean commits
- VSCode → auto-fix on save

👉 IMPORTANT:
We DO NOT use "eslint-plugin-prettier"
Prettier runs independently (best practice)

# =========================================================

# 0. (OPTIONAL) Angular ESLint via CLI

# =========================================================

# If you already ran:

ng add @angular-eslint/schematics

✔ Installs angular-eslint and related plugins
✔ Sets up ESLint builder (ng lint)
✔ Generates initial ESLint config
✔ Installs ESLint (usually)

❌ Does NOT install:

- Prettier
- eslint-config-prettier
- Husky
- lint-staged
- VSCode settings

👉 IMPORTANT:
If you used this command, DO NOT reinstall angular-eslint or eslint.

# =========================================================

# 1. 📦 Install dependencies (ONLY what is missing)

# =========================================================

npm i -D prettier eslint-config-prettier
npm i -D husky lint-staged

❌ DO NOT install:
npm i -D eslint-plugin-prettier

Reason:
We do NOT run Prettier inside ESLint.

# =========================================================

# 2. 🧹 ESLint (Flat Config)

# =========================================================

# File: eslint.config.js

# add to the end of file => eslintConfigPrettier,

const eslint = require('@eslint/js')
const { defineConfig } = require('eslint/config')
const tseslint = require('typescript-eslint')
const angular = require('angular-eslint')
const eslintConfigPrettier = require('eslint-config-prettier')

module.exports = defineConfig([
{
files: ['**/*.ts'],
extends: [
eslint.configs.recommended,
tseslint.configs.recommended,
tseslint.configs.stylistic,
angular.configs.tsRecommended,
],
processor: angular.processInlineTemplates,
rules: {
'@angular-eslint/directive-selector': [
'error',
{ type: 'attribute', prefix: 'app', style: 'camelCase' },
],
'@angular-eslint/component-selector': [
'error',
{ type: 'element', prefix: 'app', style: 'kebab-case' },
],
},
},
{
files: ['**/*.html'],
extends: [
angular.configs.templateRecommended,
angular.configs.templateAccessibility,
],
},

# Disable ESLint formatting conflicts with Prettier

eslintConfigPrettier,
])

# =========================================================

# 3. 🎨 Prettier config

# =========================================================

# File: .prettierrc

{
"printWidth": 100,
"singleQuote": true,
"semi": false,
"overrides": [
{
"files": "*.html",
"options": {
"parser": "angular"
}
}
]
}

# =========================================================

# 4. 🪝 Husky setup

# =========================================================

npx husky init

# This creates:

# .husky/pre-commit

# =========================================================

# 5. ⚙️ lint-staged config

# =========================================================

# File: package.json (To add after "scripts": {...},)

"lint-staged": {
"\*.{ts,html,scss,json}": [
"prettier --write",
"eslint --fix"
]
}

# =========================================================

# 6. 🪝 Pre-commit hook

# =========================================================

# File: .husky/pre-commit

# Add the following line =>

npx lint-staged

# =========================================================

# 7. 📜 Scripts (optional but recommended)

# =========================================================

# File: package.json

{
"scripts": {
...
"lint": "eslint .",
"lint:fix": "eslint . --fix",
"format": "prettier . --write",
"prepare": "husky"
}
}

# =========================================================

# 8. 🧠 VSCode config

# =========================================================

# File: .vscode/settings.json

{
"editor.formatOnSave": true,
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.codeActionsOnSave": {
"source.fixAll.eslint": "explicit"
}
}

# =========================================================

# 🔁 FINAL WORKFLOW

# =========================================================

# On save:

Prettier → formats code
ESLint → fixes code issues

# On commit:

lint-staged runs:

1. prettier --write
2. eslint --fix

👉 Commit is blocked if issues remain

# =========================================================

# 🏆 KEY PRINCIPLES

# =========================================================

✔ ESLint = code quality
✔ Prettier = formatting
✔ Husky = enforcement
✔ lint-staged = fast execution (only staged files)

# =========================================================

# ⚠️ COMMON MISTAKES

# =========================================================

❌ Installing eslint-plugin-prettier
❌ Running Prettier inside ESLint
❌ Reinstalling angular-eslint after ng add
❌ Mixing .eslintrc with flat config
❌ Forgetting eslint-config-prettier

# =========================================================

# 📌 COMMIT MESSAGE

# =========================================================

chore: setup eslint, prettier and husky pre-commit hooks

# 👉 Setup ready in minutes
