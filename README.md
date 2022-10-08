# Vite-Electron-React-Typescript Template

This is a template for secure electron applications, building upon [vite-electron-builder](https://github.com/cawa-93/vite-electron-builder). Please refer to that template for more detail about the app's structure and desgining mechanism. 

## Get started

Follow these steps to get started with the template:

1. Click the **[Use this template](https://github.com/amy-juan-li/electron-vite-react-typescript/generate)** button (you must be
   logged in) or just clone this repo.
2. If you want to use another package manager don't forget to edit [`.github/workflows`](/.github/workflows) -- it
   uses `npm` by default.
   
Run the app in development mode:
```bash
npm i
npm run watch
```
## How it works

The template requires a minimum amount [dependencies](package.json). Only **Vite** is used for building, nothing more.

### Project Structure

The structure of this template is very similar to the structure of a monorepo.  
**packages/main**: Electron main script.  
**packages/preload**: Used in BrowserWindow.webPreferences.preload.   
**packages/renderer**: Electron web page. 

### Build web resources

The `main` and `preload` packages are built in [library mode](https://vitejs.dev/guide/build.html#library-mode) as it is
simple javascript.
The `renderer` package builds as a regular web app.

### Compile App

The next step is to package a ready to distribute Electron app for macOS, Windows and Linux with "auto update" support
out of the box.

To do this, use [electron-builder]:

- Using the npm script `compile`: This script is configured to compile the application as quickly as possible. It is not
  ready for distribution, it is compiled only for the current platform and is used for debugging.
- Using GitHub Actions: The application is compiled for any platform and ready-to-distribute files are automatically
  added as a draft to the GitHub releases page.
  
```bash
npm run compile
# for mac user:
open dist/mac/your-app-name.app
```

### Modes and Environment Variables

All environment variables are set as part of the `import.meta`, so you can access them vie the following
way: `import.meta.env`.

If you are using TypeScript and want to get code completion you must add all the environment variables to
the [`ImportMetaEnv` in `types/env.d.ts`](types/env.d.ts).

The mode option is used to specify the value of `import.meta.env.MODE` and the corresponding environment variables files
that need to be loaded.

By default, there are two modes:

- `production` is used by default
- `development` is used by `npm run watch` script

When running the build script, the environment variables are loaded from the following files in your project root:

```
.env                # loaded in all cases
.env.local          # loaded in all cases, ignored by git
.env.[mode]         # only loaded in specified env mode
.env.[mode].local   # only loaded in specified env mode, ignored by git
```

To prevent accidentally leaking env variables to the client, only variables prefixed with `VITE_` are exposed to your
Vite-processed code.

For example let's take the following `.env` file:

```
DB_PASSWORD=foobar
VITE_SOME_KEY=123
```

Only `VITE_SOME_KEY` will be exposed as `import.meta.env.VITE_SOME_KEY` to your client source code, but `DB_PASSWORD`
will not.
