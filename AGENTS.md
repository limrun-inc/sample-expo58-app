This is an Expo/React Native mobile application. Prioritize mobile-first patterns, performance, and cross-platform compatibility.

## Expo has changed — do not trust your training data

Expo ships breaking changes every SDK release. APIs you remember are likely renamed, moved, or removed. Before writing any code that touches an Expo, EAS, or React Native API:

1. Read the major version of the `expo` package in `package.json`.
2. Fetch the matching versioned docs: `https://docs.expo.dev/versions/v<major>.0.0/`
3. For anything else, fetch https://docs.expo.dev/llms.txt — an index of all Expo docs with corrections to common LLM misconceptions. Follow its links to the specific page you need; never answer from memory.

## Commands

Use `bunx` instead of `npx` if the project uses bun (`bun.lock` present).

```bash
npx expo install <package>  # ALWAYS use instead of npm/yarn/pnpm/bun add — resolves SDK-compatible versions
npx expo start              # start the dev server
npx expo lint               # lint
npx tsc --noEmit            # typecheck
npx expo-doctor             # diagnose dependency and config issues
npx expo install --fix      # fix incompatible package versions
```

Run lint and typecheck before declaring any task done.

## Navigation & Routing

- Use **Expo Router** for all navigation. Routes live in `src/app/` — every file there is a screen, `_layout.tsx` files define navigators. Keep non-route code (components, hooks, utils) outside `src/app/`.
- Import `Link`, `router`, and `useLocalSearchParams` from `expo-router`.
- Docs: https://docs.expo.dev/router/introduction.md

## Build and test with Limrun

Build and run this app with the `lim` CLI, not EAS, local Xcode, Android Studio, or a local simulator. `lim` builds on remote Xcode and Gradle sandboxes and runs the app on cloud iOS simulators and Android emulators.

```bash
npm install --global lim
lim skills install
```

`lim` reads `LIM_API_KEY` from the environment or a `.env` file. The `limrun-expo-development` skill has the full workflow; the commands below are the short path.

iOS: the Release build includes the JavaScript bundle and runs without Metro. `--attach` installs and launches the latest build on the new simulator.

```bash
lim xcode build . --configuration Release
lim ios create --attach
```

Android: the default Debug build is a development client that connects to Metro. After these commands, follow "Build with Limrun", "Android" in `README.md` to tunnel Metro to the emulator.

```bash
lim gradle build . --upload sample-expo58-app/android-debug.apk
lim android create --install-asset sample-expo58-app/android-debug.apk --no-connect
```

Delete each instance when you finish, with `lim ios delete <id>` or `lim android delete <id>`.

## Rules

- If `ios/` and `android/` directories do not exist, they are generated (Continuous Native Generation). Never create or edit them by hand — configure native behavior in `app.json` and config plugins.
- Expo Go only includes its bundled native modules. After adding a library with native code, the app needs a development build: build it with Limrun (see "Build and test with Limrun") with `--configuration Debug` on iOS or the default Debug build on Android.
- Prefer recommended Expo modules over third-party libraries, and check your available skills before adding dependencies. Docs: https://docs.expo.dev/versions/latest/index.md
