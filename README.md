<h1 align="center">Jellyfin for Tizen</h1>
<h3 align="center">Part of the <a href="https://jellyfin.media">Jellyfin Project</a></h3>

## Build Process
_Also look [Wiki](https://github.com/jellyfin/jellyfin-tizen/wiki)._

### Prerequisites
* Tizen Studio 4.6+ with IDE or Tizen Studio 4.6+ with CLI (<a href="https://developer.tizen.org/development/tizen-studio/download">https://developer.tizen.org/development/tizen-studio/download</a>)
* Git
* Node.js 16+

### Getting Started

1. Install prerequisites.
2. Install Certificate Manager using Tizen Studio Package Manager.
3. Setup Tizen certificate in Certificate Manager.
4. Clone or download Jellyfin Web repository (<a href="https://github.com/jellyfin/jellyfin-web">https://github.com/jellyfin/jellyfin-web</a>).
   ```sh
   git clone https://github.com/jellyfin/jellyfin-web.git
   ```
5. Clone or download Jellyfin Tizen (this) repository.
   ```sh
   git clone https://github.com/jellyfin/jellyfin-tizen.git
   ```

### Build Jellyfin Web

```sh
cd jellyfin-web
SKIP_PREPARE=1 npm ci --no-audit
npm run build:production
```

> You should get `jellyfin-web/dist/` directory.

> `SKIP_PREPARE=1` can be omitted for 10.9+.

> Use `npm run build:development` if you want to debug the app.

If any changes are made to `jellyfin-web/`, the `jellyfin-web/dist/` directory will need to be rebuilt using the command above.

### Prepare Interface

```sh
cd jellyfin-tizen
JELLYFIN_WEB_DIR=../jellyfin-web/dist npm ci --no-audit
```

> You should get `jellyfin-tizen/www/` directory.

> The `JELLYFIN_WEB_DIR` environment variable can be used to override the location of `jellyfin-web`.

If any changes are made to `jellyfin-web/dist/`, the `jellyfin-tizen/www/` directory will need to be rebuilt using the command above.

### Build WGT

> Make sure you select the appropriate Certificate Profile in Tizen Certificate Manager. This determines which devices you can install the widget on.

```sh
tizen build-web -e ".git*" -e gulpfile.js -e README.md -e "node_modules/*" -e "package*.json" -e "yarn.lock"
tizen package -t wgt -o . -- .buildResult
```

> You should get `Jellyfin.wgt`.

## Deployment

### Deploy to Emulator

1. Run emulator.
2. Install package.
   ```sh
   tizen install -n Jellyfin.wgt -t T-samsung-5.5-x86
   ```
   > Specify target with `-t` option. Use `sdb devices` to list them.

### Deploy to TV

1. Run TV.
2. Activate Developer Mode on TV (<a href="https://developer.samsung.com/tv/develop/getting-started/using-sdk/tv-device">https://developer.samsung.com/tv/develop/getting-started/using-sdk/tv-device</a>).
3. Connect to TV with Device Manager from Tizen Studio. Or with sdb.
   ```sh
   sdb connect YOUR_TV_IP
   ```
4. If you are using a Samsung certificate, `Permit to install applications` on your TV using Device Manager from Tizen Studio. Or with sdb.
   > TODO: Find a command
5. Install package.
   ```sh
   tizen install -n Jellyfin.wgt -t UE65NU7400
   ```
   > Specify target with `-t` option. Use `sdb devices` to list them.
