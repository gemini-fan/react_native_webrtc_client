# react_native_webrtc_client
## 🚀 Development
### 1. Install dependencies
- Go to root directory and run
```
npm install
```
### 2. Configure your socket connection in `App.js`
```
const socket = SocketIOClient("http://192.168.0.123:3500", {});
```
### 3. Run the Metro
```
npm run start
```
### 4. Connect Android Device
- Pre-requisite: Spent 1 hour on the [react-native website](https://reactnative.dev/docs/set-up-your-environment) for environmetn setup
- Replace `04e8` with your device USB vendor ID
```
echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", MODE="0666", GROUP="plugdev"' | sudo tee /etc/udev/rules.d/51-android-usb.rules
adb devices
```
### 5. Run on device/emulator
```
npm run android
# or
npm run ios
```
---

## 📦 Build APK
### 1. Generate JS bundle

First, ensure assets folder exists:
```
mkdir -p android/app/src/main/assets
```

Bundle React Native JS:
```
npx react-native bundle \
  --platform android \
  --dev false \
  --entry-file index.js \
  --bundle-output android/app/src/main/assets/index.android.bundle \
  --assets-dest android/app/src/main/res/
```

### 2. Build APK

From inside android/:
```
cd android
./gradlew assembleRelease
```

APK will be generated at:

android/app/build/outputs/apk/release/app-release.apk

#### 🔑 Keystore & Signing

Current configuration in android/app/build.gradle:
```
signingConfigs {
    debug {
        storeFile file('debug.keystore')
        storePassword 'android'
        keyAlias 'androiddebugkey'
        keyPassword 'android'
    }
}
buildTypes {
    debug {
        signingConfig signingConfigs.debug
    }
    release {
        signingConfig signingConfigs.debug
        minifyEnabled enableProguardInReleaseBuilds
        proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
    }
}
```

##### ⚠️ For production, generate your own keystore:
```
keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

- Update build.gradle to point to your release keystore.