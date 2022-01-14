# How to Deploy onixinfo

onixinfo is splitted into 3 repos:
* [https://github.com/onixcoin-io/onixinfo](https://github.com/onixcoin-io/onixinfo)
* [https://github.com/onixcoin-io/onixinfo-api](https://github.com/onixcoin-io/onixinfo-api)
* [https://github.com/onixcoin-io/onixinfo-ui](https://github.com/onixcoin-io/onixinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy onix core
1. `git clone --recursive https://github.com/onixcoin-io/onix.git --branch=onixinfo`
2. Follow the instructions of [https://github.com/onixcoin-io/onix/blob/master/README.md#building-onix-core](https://github.com/onixcoin-io/onix/blob/master/README.md#building-onix-core) to build onix
3. Run `onixd` with `-logevents=1` enabled

## Deploy onixinfo
1. `git clone https://github.com/onixcoin-io/onixinfo.git`
2. `cd onixinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `onixinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `onixinfo` under a process manager (like `pm2`), to restart the process when `onixinfo` crashes.

## Deploy onixinfo-api
1. `git clone https://github.com/onixcoin-io/onixinfo-api.git`
2. `cd onixinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy onixinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/onixcoin-io/onixinfo-ui.git`
2. `cd onixinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "ONIXINFO_API_BASE_CLIENT=/api/ ONIXINFO_API_BASE_SERVER=http://localhost:3001/ ONIXINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `onixinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
