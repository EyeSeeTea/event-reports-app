# event-reports-app
DHIS 2 app for event data visualization

### Setup

Check the branch with the version you intend to use.
If the app change needs to update `d2-analysis` too, before installing the packages,  pull the repository [d2-analysis](https://github.com/EyeSeeTea/d2-analysis) and check the build of the appropriate version in the same folder as this repository.

```sh
grep -A 2 "^d2-analysis@" yarn.lock | grep "version"
# Example output:  version "33.3.4"
```

In `package.json` change:
```json
-    "d2-analysis": "^33.3.4",
+    "d2-analysis": "../d2-analysis",
```

Then:

```sh
cd ..
git clone git@github.com:EyeSeeTea/d2-analysis.git
cd d2-analysis
git checkout -b <version> refs/remotes/origin/<version>
yarn install
yarn build
```

With the dependencies met run:
```sh
nvm install
nvm use

yarn install
```


### Start

To use `yarn start` set your test instance href and credentials in the `manifest.webapp` section of `package.json`.

If you need to update `d2-analysis`code too you can either:
- Build `d2-analysis` and run `yarn add ../d2-analysis` to update the package.

- Use these commands (or add them as yarn commands):
    ```sh
    # link d2-analysis
    cd ../d2-analysis && yarn link && cd - && yarn link d2-analysis

    # unlink d2-analysis
    yarn unlink d2-analysis && cd ../d2-analysis && yarn unlink && cd -

    # watch-d2-analysis
    cd ../d2-analysis && yarn run build --watch
    ```

    And add this to `webpack.config.js` devServer config:

    ```js
    webpackBaseConfig.devServer = {
    ...
    watchOptions: {
        poll: 1000,
        aggregateTimeout: 300,
        ignored: /node_modules(?!\/d2-analysis)/,
    },
    };
    ```

    With this `webpack-dev-server` will reload if `d2-analysis` build updates.

    So, after linking and starting the watch build you can run `yarn start` and have `d2-analysis` hot-swap.


### Notes
Workaround pngquant-bin issue on install:

- `sudo apt-get install libpng-dev`
- `yarn global add pngquant-bin`
