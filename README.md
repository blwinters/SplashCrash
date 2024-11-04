# SplashCrash

This is a minimal crash reproduction, created with [`create-expo-app`](https://www.npmjs.com/package/create-expo-app).

It seems that adding `expo-dev-client` to a vanilla Expo project causes a crash in local builds due to not finding the splash screen. I'm not sure when this was introduced, but it exists in v4.0.28.

## Reproduction steps

1. `npm install`
2. `npx expo prebuild`
3. `npx expo run:ios` (This should run fine)
4. `ctrl-c` to stop the app
5. `npx expo run:ios` (This will crash with the error below)

## Removing the crash

After reproducing the crash,

1. Remove `expo-dev-client` from `package.json`
2. `npm install`
3. `npx expo prebuild --clean`
4. Delete `DerivedData` (`~/Library/Developer/Xcode/DerivedData/SplashCrash*`)
5. Delete app from simulator
6. `npx expo run:ios` (This should now run fine multiple times in a row)

## Error

``` bash
[CoreFoundation] *** Terminating app due to uncaught exception 'ERR_NO_SPLASH_SCREEN', reason: 'Couln't locate neither
'SplashScreen.storyboard' file nor 'SplashScreen.xib' file. Create one of these in the project to make
'expo-splash-screen' work (https://github.com/expo/expo/tree/main/packages/expo-splash-screen#-configure-ios).'
*** First throw call stack:
(
0   CoreFoundation                      0x00000001804b757c __exceptionPreprocess + 172
1   libobjc.A.dylib                     0x000000018008eda8 objc_exception_throw + 72
2   SplashCrash.debug.dylib             0x00000001039858a8 -[EXSplashScreenViewNativeProvider createSplashScreenView] +
1588
3   SplashCrash.debug.dylib             0x0000000103983430 -[EXSplashScreenService
showSplashScreenFor:options:splashScreenViewProvider:successCallback:failureCallback:] + 424
4   SplashCrash.debug.dylib             0x00000001039831e8 -[EXSplashScreenService showSplashScreenFor:options:] + 128
5   SplashCrash.debug.dylib             0x0000000103984360 -[EXSplashS<…>
```
