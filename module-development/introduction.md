---
title: Introduction
sort: 1
---

# Starting out making GooseMod modules

## Module structure
### Manifest file

The manifest file contains the metadata for your module. Its name is `goosemodModule.json`.

```json
{
  "main": "",

  "name": "",
  "description": "",
  "tags": ["", ""],

  "authors": ["", ""],

  "version": ""
}
```

- `main`: Main file of your module.
- `name`: Name of your module.
- `description`: Description of your module.
- `tags`: List of tags for your module.
- `authors`: List of authors (Discord IDs).
- `version`: Version of your module (semver).

### Main

Your main file will probably have your main definitions, but the only mandatory definition is the exported object:

```js
export default {
  gooseHandlers: {
    onImport: () => {
      // Will be ran when your module is installed or on every GM load.
    },
    onRemove: () => {
      // Will be ran when your module is uninstalled or disabled.
      // Clean up everything here.
    },
  },
};
```

### Other files

You can separate your code in multiple files using ES module syntax.

## Testing your module

Once you have reached a point when you want to test your module, you'll want to run the development server. You can get it by cloning the [GooseMod/MS2Builder](https://github.com/GooseMod/MS2Builder) github repository.

You can then run the developement server with the following command, in the repository's directory:  
`node src/dev.js <modules_dir> <port>`, replacing `<modules_dir>` with the directore in which you have your modules and `<port>` with the port to serve on.

An exemple `<modules_dir>` structure would be:

- `modules/`
  - `module-1/`
    - `goosemodModule.json`
    - `index.js`
  - `module-2/`
    - `goosemodModule.json`
    - `main.js`
    - ...
  - ...

## Local modules

```note
Local modules are currently a non-supported feature, but you could use [this method](#custom-repository) to have your own repository, or use the [development](#testing-your-module) server, although you need to launch it every time.
```

## Publishing your modules
### Official repository

Publishing to the official repository can be asked for on the [GooseNest Discord server](https://goosemod.com/discord).

### Custom repository

You can make your own repository by taking [GooseMod/MS2Builder](https://github.com/GooseMod/MS2Builder) as a base.

The first step is to clean up the repository definitions. Remove the files you don't need from the followin and remove all entries from those you kept:

- `src/modules/goosemod.js`: GooseMod modules (you will probably want to keep this one, just saying).
- `src/modules/ms2porter.js`: Ported GM MS1 modules (you won't need this one).
- `src/modules/ports/plugins/pcPlugins.js`: Powercord plugins auto-ports.
- `src/modules/ports/themes/bdThemes.js`: BetterDiscord themes auto-ports.
- `src/modules/ports/themes/pcThemes.js`: Powercord themes auto-ports.

To finish cleaning up, edit `src/modules/index.js`. This file contains the metadata for each generated repository. Remove the entries related to the files you previously removed, as well as the related imports at the top of the file.

Now that you've cleared everything unneeded out, you can start setting your repository up:

- Edit the module lists in the files you kept. The format for a single module is:
```js
[
  'Github/repository',                            // Example: 'GooseMod/GMExampleModule'
  'ref (commit hash, branch or tag)',             // Example: 'main' (recommended to use a commit hash for safety,
                                                  //                  if left clear uses the default branch)
  'path to module',                               // [OPTIONAL] Example: '/path/to/module'
  'preprocessor (undefined for GM modules)',      // [OPTIONAL] Example: 'pcPlugin' (used for autoports)
  {}                                              // [OPTIONAL] Metadata override (see goosemodModule.json)
]
```
- Edit the repository metadata in `src/modules/index.js`:
  - `meta`:
    - `name`: Name of the repository shown in the repository list in GooseMod.
    - `description`: Description in the repository shown in the repository list in GooseMod.
  - `filename`: Name of the generated repository definition file.
  - `modules`: Object containing module definitions, use imports to keep the file clean.

Add a file named `gh_pat.js` in the `src/` directory, with the following content:
```js
export default "ghp_randomstuff"; // replace with an actual Github Personal Access Token
```

You can now buid your repository/repositories by running `node src/index.js` from the base directory of the MS2Builder clone. 

If all went well, there should have been no errors in the building process. If there were errors you can ask for support on the [GooseNest Discord server](https://goosemod.com/discord), on the `#module-dev` channel.

You now have the necessary files for hosting the repository in `dist/`. You can just serve the directory directly, or you could make a repository like `GooseMod/MS2Builder`'s to host your repositories and built files, or even go a step further and setup CI/CD. That's all on you though.