# Webpack 5 test repo

This repo shows some edge cases when build fails

Note: Issue fixed at https://github.com/webpack/webpack/releases/tag/v5.10.0

## How to reproduce

1. Clone this repo
2. Run `npm ci`
3. Run `cd webpack4 && npm ci && npm run build`

    The output should look smth like this:
    ```
    [webpack-cli] Compilation finished
    Hash: 97e4e80c7a9c34b4ebe6
    Version: webpack 4.44.2
    Time: 394ms
    Built at: 12/03/2020 5:54:14 PM
    Asset      Size  Chunks             Chunk Names
    main.js  2.23 KiB       0  [emitted]  main
    Entrypoint main = main.js
    [0] ../src/node_modules/@test/base/index.es.js + 1 modules 10.1 KiB {0} [built]
        | ../src/node_modules/@test/base/index.es.js 86 bytes [built]
        | ../node_modules/tslib/tslib.es6.js 10 KiB [built]
    [1] ../src/index.js 32 bytes {0} [built]
    [2] ../src/b.js 47 bytes {0} [built]
    [3] ../src/a.js + 1 modules 89 bytes {0} [built]
        | ../src/a.js 53 bytes [built]
        | ../src/node_modules/@test/reexport/index.js 31 bytes [built]
    ```

    And there should be generated dist directory with bundle
4. Run `cd ../webpack5 && npm ci && npm run build`

    The last command will fail:
    ```
    [webpack-cli] Compilation finished
    assets by status 976 bytes [cached] 1 asset
    runtime modules 668 bytes 3 modules
    orphan modules 10.1 KiB [orphan] 2 modules
    cacheable modules 10.2 KiB
    ../src/index.js 32 bytes [built] [code generated]
    ../src/a.js 53 bytes [built] [code generated]
    ../src/b.js 47 bytes [built] [code generated]
    ../src/node_modules/@test/base/index.es.js + 1 modules 10.1 KiB [built] [code generated]

    ERROR in ../src/a.js
    RuntimeTemplate.importStatement(): Module /workspaces/open-source/webpack5-require-bug/src/node_modules/@test/reexport/index.js has no id assigned.
    This should not happen.
    It's in these chunks: none (If module is in no chunk this indicates a bug in some chunk/module optimization logic)
    Module has these incoming connections:
    - /workspaces/open-source/webpack5-require-bug/src/a.js harmony side effect evaluation
    - /workspaces/open-source/webpack5-require-bug/src/a.js harmony import specifier
    @ ../src/index.js 1:0-14

    webpack 5.9.0 compiled with 1 error in 428 ms
    ```

    And no bundle will be outputted