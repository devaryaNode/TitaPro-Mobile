# TitaPro - Command Cheat Sheet

Copied from the project's main README. Android release APKs are attached to this repository's Releases.

Every `npm run` command in this repo, in plain words. Run them from the project root unless a section says otherwise. Commands marked 🪟 are written for Windows.

### Run the app while developing

| Command | What it does | When to use it |
|---|---|---|
| `npm start` | Starts Metro, the JavaScript bundler | Keep it running in one terminal while you work |
| `npm run android` | Builds the debug app and installs it on the connected Android phone or emulator | First run, or after a native change |
| `npm run ios` | Builds and runs the app on the iOS simulator (macOS only) | iOS testing |
| `npm run adb:log` | Shows the phone's live log, only React Native lines | Debugging on a phone connected by USB |
| `npm run kill:node` 🪟 | Force-closes every Node process | Metro is stuck or a port is busy |

Tip: if the app shows old code, restart Metro with `npm start -- --reset-cache`.

### Build the Android release APK

| Command | What it does | When to use it |
|---|---|---|
| `npm run build:android` 🪟 | Builds the signed release APK at `android/app/build/outputs/apk/release/app-release.apk`. Does not change the version number | A release build to test or share |
| `npm run build-install:android` 🪟 | Builds the release APK, installs it on the connected phone and opens the app | Checking a release build on a real phone |
| `npm run clean:android` 🪟 | Deletes old Android build output | After native or Gradle changes, or odd build errors |

The Play Store bundle (AAB) has no npm command: run `cd android` then `.\gradlew bundleRelease`.

### Publish the Android APK to GitHub

| Command | What it does | When to use it |
|---|---|---|
| `npm run release:android:github` | Builds the release APK, then publishes it | The normal "ship it" command |
| `npm run publish:android:github` | Publishes the APK that is already built | The APK is built and only the upload is left |
| `node scripts/android-github-publish.js --dry-run` | Checks everything and changes nothing on GitHub | Before the first real publish |
| `npm run publish:readme:github` | Puts a copy of this cheat sheet, and nothing else from this README, on the releases repo as its `README.md` | After you change this cheat sheet |

- It publishes to `devaryaNode/TitaPro-Mobile` as release `android-v<versionName>-<versionCode>` (for example `android-v2.0-17`). Change the target with `GITHUB_OWNER` and `GITHUB_REPO`.
- Raise `versionCode` in `android/version.properties` first when testers must install the new build over the old one. Publishing the same version again replaces that release's APK.
- You need to be logged in with `gh auth login` as the account that owns the repo (or set `GITHUB_TOKEN`, for example in the git-ignored `.env.publish`).
- The first publish into an empty releases repo adds that same cheat-sheet `README.md` first, because GitHub cannot hold a release in a repo with no commits. The rest of this README (internal hosts, endpoints) is never published there.
- The desktop app's own releases are separate and are never touched.

### Test and check the code

| Command | What it does | When to use it |
|---|---|---|
| `npm test` | Runs all Jest tests | Before you commit |
| `npm test -- <file>` | Runs one test file, for example `npm test -- __tests__/groupAdmin.test.ts` | Working on one feature |
| `npm run lint` | Checks code style with ESLint | Before you commit |
| `npm run docs:check` | Checks that the notes (`AGENTS.md` and the `.brain` wiki) still match the code | Before you push |
| `npm run docs:report` | Same check plus a table of which notes are behind | Finding notes that need updating |
| `npm run docs:setup` | Turns on the git hook that runs the docs check before every push | Once per clone |

### Desktop (Electron) app

These run from the project root and forward into the `electron/` folder.

| Command | What it does | When to use it |
|---|---|---|
| `npm run electron:setup` | Installs the Electron packages and builds the web bundle | First time only |
| `npm run electron:dev` | Starts the Vite dev server with live reload | Building the desktop UI |
| `npm run electron:start` | Opens the Electron window | After `electron:dev` is running, or after a build |
| `npm run electron:dist` | Builds the web bundle and the installer with electron-builder | A desktop installer |
| `npm run electron:pack` | Builds a Windows x64 app folder (no installer) in `electron/release` | A quick unpacked desktop build |

Inside the `electron/` folder the short names also work: `npm run dev`, `build`, `preview`, `start`, `dist` and `pack`.

### Runs by itself

`npm install` automatically runs `postinstall`, which applies the repo's patches and compiles the background-location package if needed. You never run it by hand.
