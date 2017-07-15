![Hotdog or Not?](/screenshots/nhd-icon.png?raw=true)

# Hotdog or not?

Ever wondered if a thing is a hotdog or not? Neither did I...

Anyway, now you have a sure way of determining the hotdogness of a thing simply
by taking a picture and pressing the "Hotdog or Not?" button!

At the time of writing, the app does not work, though due to weird crashes :(...

## Purpose

This app was built to test the [nativescript-vue](https://github.com/rigor789/nativescript-vue) plugin and to participate in the
NativeScript's [2017 Sweetness and Light Summer App Contest!](https://www.nativescript.org/blog/2017-sweetness-and-light-summer-app-contest).

## Screenshots

![Android version](/screenshots/hotdog-or-not-android.png?raw=true "Android version")
-
![iOS version](/screenshots/hotdog-or-not-ios.png?raw=true "iOS version")

## Status

- [x] App built with Vue and NativeScript.
- [x] Uses single file components (sort of...).
- [ ] Code does not embarass its creator.
- [ ] Works in Android.
- [ ] Works in iOS.
- [x] Awesome icon!

## Usage

1. Clone this repository
```bash
git clone https://github.com/tralves/not-hotdog-ns-vue
```

2. Install dependencies
```bash
cd /not-hotdog-ns-vue
yarn
```
4. Run in Android or iOS (see: [{NS} documentation on webpack bundling](https://docs.nativescript.org/tooling/bundling-with-webpack#bundling))
```
npm run start-android-bundle -- --clean
npm run start-ios-bundle -- --clean
```