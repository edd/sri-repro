Repository created to demonstrate issue [#10926](https://github.com/nrwl/nx/issues/10926)

# Notes
- This is a new workspace generated with `nx@latest` (currently `14.3.6`)
- There is one application in the workspace, a React application
- SRI is enabled in that project.json
- Integrity attributes are generated in dev, but not production

# Logs

## Environment
```
❯ npx nx report

 >  NX   Report complete - copy this into the issue template

   Node : 18.4.0
   OS   : linux x64
   npm  : 8.12.1
   
   nx : 14.3.6
   @nrwl/angular : Not Found
   @nrwl/cypress : 14.3.6
   @nrwl/detox : Not Found
   @nrwl/devkit : 14.3.6
   @nrwl/eslint-plugin-nx : 14.3.6
   @nrwl/express : Not Found
   @nrwl/jest : 14.3.6
   @nrwl/js : 14.3.6
   @nrwl/linter : 14.3.6
   @nrwl/nest : Not Found
   @nrwl/next : Not Found
   @nrwl/node : Not Found
   @nrwl/nx-cloud : Not Found
   @nrwl/nx-plugin : Not Found
   @nrwl/react : 14.3.6
   @nrwl/react-native : Not Found
   @nrwl/schematics : Not Found
   @nrwl/storybook : 14.3.6
   @nrwl/web : 14.3.6
   @nrwl/workspace : 14.3.6
   typescript : 4.7.4
   ---------------------------------------
   Community plugins:

```
## Creating the app
```
❯ npx create-nx-workspace@latest sri-repro
✔ What to create in the new workspace · react
✔ Application name                    · sri-repro-app
✔ Default stylesheet format           · css
✔ Use Nx Cloud? (It's free and doesn't require registration.) · No

 >  NX   Nx is creating your v14.3.6 workspace.

   To make sure the command works reliably in all environments, and that the preset is applied correctly,
   Nx will run "npm install" several times. Please wait.

✔ Installing dependencies with npm
✔ Nx has successfully created the workspace.

 ————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————


 >  NX   Nx CLI is not installed globally.

   This means that you might have to use "yarn nx" or "npx nx" to execute commands in the workspace.
   Run "yarn global add nx" or "npm install -g nx" to be able to execute command directly.


 ————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————


 >  NX   First time using Nx? Check out this interactive Nx tutorial.

   https://nx.dev/react-tutorial/01-create-application
   
   Prefer watching videos? Check out this free Nx course on Egghead.io.
   https://egghead.io/playlists/scale-react-development-with-nx-4038


```
### Enabling SRI
https://github.com/edd/sri-repro/commit/3f60a8a5fc67a1fc8f8f5ad30c46e07d5f10cf31

### Checking the dev output 
```
❯ npx nx serve sri-repro-app

> nx run sri-repro-app:serve:development

<i> [webpack-dev-server] Project is running at:
<i> [webpack-dev-server] Loopback: http://localhost:4200/, http://127.0.0.1:4200/
<i> [webpack-dev-server] 404s will fallback to '/index.html'

>  NX  Web Development Server is listening at http://localhost:4200/

Entrypoint main [big] 1.6 MiB (1.88 MiB) = runtime.js 51.1 KiB vendor.js 1.49 MiB main.js 57.6 KiB 3 auxiliary assets
Entrypoint polyfills [big] 764 KiB (927 KiB) = runtime.js 51.1 KiB polyfills.js 713 KiB 2 auxiliary assets
Entrypoint styles [big] 431 KiB (535 KiB) = runtime.js 51.1 KiB styles.css 399 bytes styles.js 380 KiB 2 auxiliary assets
chunk (runtime: runtime) main.js (main) 49.3 KiB [initial] [rendered]
chunk (runtime: runtime) polyfills.js (polyfills) 664 KiB [initial] [rendered]
chunk (runtime: runtime) runtime.js (runtime) 34.3 KiB [entry] [rendered]
chunk (runtime: runtime) styles.css, styles.js (styles) 363 KiB (javascript) 398 bytes (css/mini-extract) [initial] [rendered]
chunk (runtime: runtime) vendor.js (vendor) (id hint: vendor) 1.48 MiB [initial] [rendered] split chunk (cache group: vendor) (name: vendor)
webpack compiled successfully (f4e54bc1abd2035d)
No issues found.
```

### CSS has `integrity` attribute

```html
 <link rel="stylesheet" href="styles.css" crossorigin="anonymous" integrity="sha384-nUzkvqwyJ6jeTLNysFiDHKdNa/+5CQyb41hZPokBdUthcmaDenM9WSKKoDD6BX+J">
```

#### JS bundle has `integrity` output
```html
 <script src="runtime.js" crossorigin="anonymous" type="module" integrity="sha384-OwuvVAWsJSLE2n/Gk1wKxgzRB/sR532KBnBj0mVK3IuhuHKGytFjb2tRW9Nx+Rau"></script>
```

### Building
 ```bash
 npx nx build sri-repro-app`
 ```
 
 #### CSS & JS missing `attribute` tags
 ```
cat dist/apps/sri-repro-app/index.html 
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>SriReproApp</title>
    <base href="/">
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="icon" type="image/x-icon" href="favicon.ico" />
  <link rel="stylesheet" href="styles.ef46db3751d8e999.css" crossorigin="anonymous"></head>
  <body>
    <div id="root"></div>
  <script src="runtime.c1411f2a2fb6c6b8.esm.js" crossorigin="anonymous" type="module"></script><script src="polyfills.cff750c80658892a.esm.js" crossorigin="anonymous" type="module"></script><script src="main.db65b37a4bbd955b.esm.js" crossorigin="anonymous" type="module"></script></body>
</html>

 ```

