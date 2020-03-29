## Quasar/vue tutorial resources
* https://quasar.dev/quasar-cli/cli-documentation/lazy-loading


## Description of Quasar Directory Structure

├── src/
│   ├── assets/              # dynamic assets (processed by webpack)
│   ├── statics/             # pure static assets (directly copied)
│   ├── components/          # .vue components used in pages & layouts
│   ├── css/                 # CSS/Stylus/Sass/... files for your app
|   |   ├── app.styl
|   │   └── quasar.variables.styl # Quasar Stylus variables for you to tweak
│   ├── layouts/             # layout .vue files
│   ├── pages/               # page .vue files
│   ├── boot/                # boot files (app initialization code)
│   ├── router/              # Vue Router     
|   |   ├── index.js         # Vue Router definition
|   │   └── routes.js        # App Routes definitions
│   ├── store/               # Vuex Store
|   |   ├── index.js         # Vuex Store definition
|   │   ├── <folder>         # Vuex Store Module...
|   │   └── <folder>         # Vuex Store Module...
│   ├── App.vue              # root Vue component of your App
│   └── index.template.html  # Template for index.html
├── src-ssr/                 # SSR specific code (like production Node webserver)
├── src-pwa/                 # PWA specific code (like Service Worker)
├── src-cordova/             # Cordova generated folder used to create Mobile Apps
├── src-electron/            # Electron specific code (like "main" thread)
├── dist/                    # where production builds go
│   ├── spa/                 # example when building SPA
│   ├── ssr/                 # example when building SSR
│   ├── electron/            # example when building Electron
│   └── ....
├── quasar.conf.js           # Quasar App Config file
├── babel.config.js          # Babeljs config
├── .editorconfig            # editor config
├── .eslintignore            # ESlint ignore paths
├── .eslintrc.js             # ESlint config
├── .postcssrc.js            # PostCSS config
├── .stylintrc               # Stylus lint config
├── .gitignore               # GIT ignore paths
├── package.json             # npm scripts and dependencies
└── README.md                # readme for your website/App