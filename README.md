# cordova_sample
example on how to convert a simple website to a mobile cordova app

## Requirements (macOS version)

- XCode (only on macOS for iOS compilation)

- imagemagick, used in `cordova-icon` (`brew install imagemagick`)


## Initializing

- clone the repo and enter its folder

- `npm install`

- `npx cordova platform add android ios browser`

- `npx cordova prepare`

- `npm run generate-cordova-icon`

- Configure `build.json` file starting from `build.sample.json`

  `cp build.sample.json build.json`



## Local web app

- `npm start`

- open browser at [localhost:8000](http://localhost:8000)


## Android development

- You need to install JDK8

  `brew cask install caskroom/versions/java8`

- You can install android-sdk, but I suggest to install android-studio

  `brew cask install android-studio`

  launch it and download android-sdk with its internal tool

- Accept all SDK package licenses

  `$ANDROID_HOME/tools/bin/sdkmanager --license`

- You need to install gradle

  `brew install gradle`

- You should give gradle execution permissions

  `sudo chmod -R 755 /Applications/Android\ Studio.app/Contents/gradle`

- Make sure to have android-sdk installed and configured, then create a keystore with Android Studio or use an existent keystore file and place it in `./certs/android/my-release-key.jks`

- Configure `build.json` file

  ```json
  {
    // ...
    "android": {
      "release": {
        "keystore": "certs/android/my-release-key.jks",
        "storePassword": "", // android keystore store password
        "alias": "", // android keystore alias
        "password": "", // android keystore password
        "keystoreType": "JKS"
      }
    }
  }
  ```


### Build

- `npm run build-android`

### Launch

- `npm run start-android`

### Release

- `npm run dist-android`

- Now you can find the Android's release apk in the following path

  `platforms/android/build/outputs/apk/android-release.apk`


## iOS development

- Make sure to have an updated version of Xcode

- I suggest you to "Automatically manage signing" of Xcode. If you want to manage compilation programmatically you should install and configure certificates for iOS development, but **ONLY** if you want to manage compilation programmatically.

  - Create the required certificates on [Apple Developer website](https://developer.apple.com/account/ios/certificate/), download and install them.
    - iPhone Developer
    - iPhone Distribution
    - Developer ID Application

  - Create the required provisioning profiles on [Apple Developer website](https://developer.apple.com/account/ios/profile/) and download them.
    - Development
    - Distribution

  - Use `security find-identity -v` to show installed Apple identities

    ```bash
    $ security find-identity -v
      1) 0000000000000000000000000000000000000000 "iPhone Developer: Name Surname (ABC1234567)"
      2) 1111111111111111111111111111111111111111 "iPhone Distribution: Name Surname (DEF1234567)"
      3) 2222222222222222222222222222222222222222 "Developer ID Application: Name Surname (DEF1234567)"
         3 valid identities found
    ```

  - Open provisioning profile files to read provisioning profile ID

    ```xml
    <key>UUID</key>
    <string>11111111-2222-3333-4444-555555555555</string>
    <!--    ^ this code is the provisioning profile id -->
    ```

  - Configure `build.json` file, but **ONLY** if you want to manage compilation programmatically.

    ```json
    {
      "ios": {
        "debug": {
          "codeSignIdentity": "0000000000000000000000000000000000000000", // iPhone Developer identity code
          "provisioningProfile": "11111111-2222-3333-4444-555555555555", // Development provisioning profile id
          "developmentTeam": "ABC1234567", // iPhone Developer team code
          "packageType": "development"
        },
        "release": {
          "codeSignIdentity": "1111111111111111111111111111111111111111", // iPhone Distribution identity
          "provisioningProfile": "e38c531c-4b77-4e4a-ae15-45727ae5e1aa", // Distribution provisioning profile id
          "developmentTeam": "DEF1234567", // iPhone Distribution team code
          "packageType": "app-store"
        }
      }
      // ...
    }
    ```


### Build

- `npm run build-ios`

### Launch

- `npm run start-ios`

### Release

- `npm run dist-ios`

- To upload the iOS app, open Xcode with `npm run open-xcode`, then open `Product` > `Archive` in the main menu to compile and upload to the App Store


## Tips

#### compile without killing the CPU

`nice -n12 npm run dist`
