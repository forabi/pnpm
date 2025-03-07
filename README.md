# pnpm

> **P**erformant **npm**

pnpm is a fast implementation of `npm install`. It is loosely based off [ied].

![](https://raw.githubusercontent.com/rstacruz/pnpm/gh-pages/screencast.gif)

[![Status](https://travis-ci.org/rstacruz/pnpm.svg?branch=master)](https://travis-ci.org/rstacruz/pnpm "See test builds")

## Install

Install it via npm.

```
npm install -g pnpm.js
```

Use `pnpm` in place of `npm`. It overrides `pnpm i` and `pnpm install`—all other commands will passthru to `npm`.

```
pnpm install lodash
```

## Preview release

`pnpm` will stay in `<1.0.0` until it's achieved feature parity with `npm install`.

- [ ] `pnpm install`
  - [x] npm packages
  - [ ] GitHub-hosted packages (`npm i rstacruz/scourjs`)
  - [x] @scoped packages (`npm i @rstacruz/tap-spec`)
  - [x] tarball release packages (`npm i http://foo.com/tar.tgz`)
  - [x] compiled packages (`npm i node-sass`)
  - [x] bundled dependencies (`npm i fsevents@1.0.6`)
  - [ ] file packages (`npm i file:../path`)
  - [x] bin executables
  - [ ] `--global` installs
  - [ ] `--save` et al
- [ ] `pnpm uninstall`
- [ ] `pnpm ls`

## Design

`pnpm` maintains a flat storage of all your dependencies in `node_modules/.store`. They are then symlinked whereever they're needed.
This is like `npm@2`'s recursive module handling (without the disk space bloat), and like `npm@3`s flat dependency tree (except with each module being predictably atomic).
To illustrate, an installation of [chalk][]@1.1.1 may look like this:

```
.
└─ node_modules/
   ├─ .store/
   │  ├─ chalk@1.1.1/
   │  │  └─ node_modules/
   │  │     ├─ ansi-styles      -> ../../ansi-styles@2.1.0
   │  │     ├─ has-ansi         -> ../../has-ansi@2.0.0
   │  │     └─ supports-color   -> ../../supports-color@2.0.0
   │  ├─ ansi-styles@2.1.0/
   │  ├─ has-ansi@2.0.0/
   │  └─ supports-color@2.0.0/
   └─ chalk                     -> .store/chalk@1.1.1
```

[chalk]: https://github.com/chalk/chalk

## Benchmark

```
time npm i babel-preset-es2015 browserify chalk debug minimist mkdirp
    66.15 real        15.60 user         3.54 sys

time pnpm i babel-preset-es2015 browserify chalk debug minimist mkdirp
    11.04 real         6.85 user         2.85 sys
```

## Prior art

[ied][] is built on a very similar premise. `pnpm` takes huge inspiration from ied.

Unlike ied, however:

- `pnpm` will eventually be made to support a globally-shared store so you can keep all your npm modules in one place. With this goal in mind, `pnpm` also doesn't care much about `npm@3`'s flat dependency tree style.
- pnpm also supports circular dependencies.

[ied]: https://github.com/alexanderGugel/ied

## Limitations

- Windows is [not supported](https://github.com/rstacruz/pnpm/issues/6).
- You can't [shrinkwrap][].
- `node_modules` managed by pnpm can't be queried properly by [npm ls][].
- You can't use [npm prune][] or [npm dedupe][].
- Things not ticked off in the [to do list](#preview-release) are obviously not feature-complete.

Got an idea for workarounds for these issues? [Share them.](https://github.com/rstacruz/pnpm/issues/new)

[shrinkwrap]: https://docs.npmjs.com/cli/shrinkwrap
[npm ls]: https://docs.npmjs.com/cli/ls
[npm prune]: https://docs.npmjs.com/cli/prune
[npm dedupe]: https://docs.npmjs.com/cli/dedupe

## Thanks

**pnpm** © 2016+, Rico Sta. Cruz. Released under the [MIT] License.<br>
Authored and maintained by Rico Sta. Cruz with help from contributors ([list][contributors]).

> [ricostacruz.com](http://ricostacruz.com) &nbsp;&middot;&nbsp;
> GitHub [@rstacruz](https://github.com/rstacruz) &nbsp;&middot;&nbsp;
> Twitter [@rstacruz](https://twitter.com/rstacruz)

[MIT]: http://mit-license.org/
[contributors]: http://github.com/rstacruz/pnpm/contributors
