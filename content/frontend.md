# Frontend

-   The communication layer is achieved by the [JavaScript agent](https://www.npmjs.com/package/@dfinity/agent)

        ```SHELL
        npm i --save @dfinity/agent
        ```

        ```JS
        // Module
        import { Actor, HttpAgent } from '@dfinity/agent';
        // Node
        const DfinityAgent = require('@dfinity/agent');
        ```

    -   The agent encodes and decodes messages to the Internet computer providing `call` `query` `readState` methods to the actor

-   The [asset canister](https://github.com/dfinity/sdk/tree/master/src/canisters/frontend/ic-frontend-canister) allows `dfx` to upload static files to IC
-   React

    -   Install React and dependencies

        ```SHELL
        npm install --save react react-dom
        npm install --save-dev typescript ts-loader
        ```

    -   Update `dfx.json` depending on your project

        ```JSON
        {
        "canisters": {
            "custom_greeting": {
            "main": "src/custom_greeting/main.mo",
            "type": "motoko"
            },
            "custom_greeting_assets": {
            "dependencies": [
                "custom_greeting"
            ],
            "frontend": {
                "entrypoint": "src/custom_greeting_assets/src/index.html"
            },
            "source": [
                "src/custom_greeting_assets/assets",
                "dist/custom_greeting_assets/"
            ],
            "type": "assets"
            }
        },
        "defaults": {
            "build": {
            "args": "",
            "packtool": ""
            }
        },
        "dfx": "0.9.3",
        "networks": {
            "local": {
            "bind": "127.0.0.1:8000",
            "type": "ephemeral"
            }
        },
        "version": 1
        }
        ```

-   Modify `webpack.config.json`

    ```JS
    const path = require("path");
    const webpack = require("webpack");
    const HtmlWebpackPlugin = require("html-webpack-plugin");
    const TerserPlugin = require("terser-webpack-plugin");
    const CopyPlugin = require("copy-webpack-plugin");

    function initCanisterEnv() {
    let localCanisters, prodCanisters;
    try {
        localCanisters = require(path.resolve(
        ".dfx",
        "local",
        "canister_ids.json"
        ));
    } catch (error) {
        console.log("No local canister_ids.json found. Continuing production");
    }
    try {
        prodCanisters = require(path.resolve("canister_ids.json"));
    } catch (error) {
        console.log("No production canister_ids.json found. Continuing with local");
    }

    const network =
        process.env.DFX_NETWORK ||
        (process.env.NODE_ENV === "production" ? "ic" : "local");

    const canisterConfig = network === "local" ? localCanisters : prodCanisters;

    return Object.entries(canisterConfig).reduce((prev, current) => {
        const [canisterName, canisterDetails] = current;
        prev[canisterName.toUpperCase() + "_CANISTER_ID"] =
        canisterDetails[network];
        return prev;
    }, {});
    }
    const canisterEnvVariables = initCanisterEnv();

    const isDevelopment = process.env.NODE_ENV !== "production";

    const frontendDirectory = "custom_greeting_assets";

    const asset_entry = path.join("src", frontendDirectory, "src", "index.html");

    module.exports = {
    target: "web",
    mode: isDevelopment ? "development" : "production",
    entry: {
        // The frontend.entrypoint points to the HTML file for this build, so we need
        // to replace the extension to `.js`.
        index: path.join(__dirname, asset_entry).replace(/\.html$/, ".jsx"),
    },
    devtool: isDevelopment ? "source-map" : false,
    optimization: {
        minimize: !isDevelopment,
        minimizer: [new TerserPlugin()],
    },
    resolve: {
        extensions: [".js", ".ts", ".jsx", ".tsx"],
        fallback: {
        assert: require.resolve("assert/"),
        buffer: require.resolve("buffer/"),
        events: require.resolve("events/"),
        stream: require.resolve("stream-browserify/"),
        util: require.resolve("util/"),
        },
    },
    output: {
        filename: "index.js",
        path: path.join(__dirname, "dist", frontendDirectory),
    },

    // Depending in the language or framework you are using for
    // front-end development, add module loaders to the default
    // webpack configuration. For example, if you are using React
    // modules and CSS as described in the "Adding a stylesheet"
    // tutorial, uncomment the following lines:
    module: {
    rules: [
        { test: /\.(ts|tsx|jsx)$/, loader: "ts-loader" },
        { test: /\.css$/, use: ['style-loader','css-loader'] }
    ]
    },
    plugins: [
        new HtmlWebpackPlugin({
        template: path.join(__dirname, asset_entry),
        cache: false,
        }),
        new CopyPlugin({
        patterns: [
            {
            from: path.join(__dirname, "src", frontendDirectory, "assets"),
            to: path.join(__dirname, "dist", frontendDirectory),
            },
        ],
        }),
        new webpack.EnvironmentPlugin({
        NODE_ENV: "development",
        ...canisterEnvVariables,
        }),
        new webpack.ProvidePlugin({
        Buffer: [require.resolve("buffer/"), "Buffer"],
        process: require.resolve("process/browser"),
        }),
    ],
    // proxy /api to port 8000 during development
    devServer: {
        proxy: {
        "/api": {
            target: "http://localhost:8000",
            changeOrigin: true,
            pathRewrite: {
            "^/api": "/api",
            },
        },
        },
        hot: true,
        watchFiles: [path.resolve(__dirname, "src", frontendDirectory)],
        liveReload: true,
    },
    };
    ```

-   Add the scripts for `webpack` compilation

    ```JSON
    // package.json
    "scripts": {
        "build": "webpack",
        "prebuild": "npm run copy:types",
        "start": "webpack serve --mode development --env development",
        "prestart": "npm run copy:types",
        "copy:types": "rsync -avr .dfx/$(echo ${DFX_NETWORK:-'**'})/canisters/** --exclude='assets/' --exclude='idl/' --exclude='*.wasm' --exclude='*.most' --delete src/declarations"
    },
    ```

-   Add `tsconfig.json`

    ```JSON
    {
        "compilerOptions": {
        "target": "es2018",        /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
        "lib": ["ES2018", "DOM"],  /* Specify library files to be included in the compilation. */
        "allowJs": true,           /* Allow javascript files to be compiled. */
        "jsx": "react",            /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
        },
        "include": ["src/**/*"],
    }
    ```

-   Check this [example](https://github.com/Donald-gg/DFX-React-Template/tree/main) for more details

## Resources

-   [Frontend Docs](https://internetcomputer.org/docs/current/developer-docs/frontend/)
-   [Examples](https://github.com/dfinity/examples)
-   [Adding a stylesheet](https://internetcomputer.org/docs/current/developer-docs/frontend/add-stylesheet)
