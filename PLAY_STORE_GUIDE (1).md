# DreamTalk AI – Google Play Store Release & Setup Guide
Package Name: com.dreamtalk.ai
Target SDK: 34 (Android 14)
Min SDK: 24 (Android 7.0)

1. Firebase Setup: Add Android App, download google-services.json to android/app/
2. AdMob: Add App ID in AndroidManifest.xml (ca-app-pub-...)
3. Google Play Billing: Create 'dreamtalk_premium_monthly' and 'dreamtalk_premium_yearly'
4. Keystore generation & build:
   flutter clean && flutter pub get && flutter build appbundle --release
Output: build/app/outputs/bundle/release/app-release.aab