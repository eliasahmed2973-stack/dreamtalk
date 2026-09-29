# DreamTalk AI – Google Play Store Release & Setup Guide

**Application Details:**
- **App Title:** DreamTalk AI – Virtual Companions
- **Package Name:** `com.dreamtalk.ai`
- **Target SDK:** 34 (Android 14)
- **Minimum SDK:** 24 (Android 7.0)

---

## 1. Firebase Project Setup

1. Go to the [Firebase Console](https://console.firebase.google.com/).
2. Create a new project named `DreamTalk AI`.
3. Add an Android app with Package Name: `com.dreamtalk.ai`.
4. Download `google-services.json` and place it in `/flutter_app/android/app/google-services.json`.
5. Enable **Firebase Authentication** (Google Sign-In, Email/Password, Anonymous).
6. Enable **Cloud Firestore** and deploy `firestore.rules` using Firebase CLI:
   ```bash
   firebase deploy --only firestore:rules,firestore:indexes
   ```
7. Deploy Cloud Functions:
   ```bash
   cd functions && npm install && npm run deploy
   ```

---

## 2. Google AdMob Configuration

1. Create an account on [Google AdMob](https://admob.google.com/).
2. Add an Android App (`com.dreamtalk.ai`).
3. Create:
   - **Banner Ad Unit**: For home and profile screens.
   - **Rewarded Video Ad Unit**: For "+5 bonus messages" reward.
4. Replace the test App ID in `AndroidManifest.xml`:
   ```xml
   <meta-data
       android:name="com.google.android.gms.ads.APPLICATION_ID"
       android:value="ca-app-pub-YOUR_ADMOB_APP_ID"/>
   ```
5. Update ad unit IDs in `lib/services/ad_service.dart`.

---

## 3. Google Play Billing Setup

1. In [Google Play Console](https://play.google.com/console), open your application.
2. Go to **Monetize > Subscriptions**.
3. Create two subscription products:
   - `dreamtalk_premium_monthly`: ₹199/month (or localized equivalent).
   - `dreamtalk_premium_yearly`: ₹1,499/year.
4. Add clear subscription benefits:
   - Higher daily message limit (1,000 messages/day).
   - Unlocked premium companions (e.g. Mia).
   - Voice conversations with natural speech synthesis.
   - Completely ad-free experience.

---

## 4. Google Play Content Rating & Data Safety Compliance

### Content Rating:
- **IARC Rating:** Teen / Mature (18+ gate enabled).
- **Fictional AI Notice:** Disclose that all characters are artificial, AI-generated personalities.
- **Violence/Sexual Content:** Zero tolerance for NSFW/CSAM/harassment. Server-side moderation filters all input and output.

### Data Safety Form:
- **Data Collected:**
  - Name and Email (for account creation & subscription sync).
  - User ID and in-app purchase history (for Google Play Billing).
  - App diagnostics and anonymous interaction analytics.
- **Data Shared:** None. User data is never sold or transferred to 3rd-party data brokers.
- **Encryption:** All network requests use TLS/HTTPS encryption.

---

## 5. Release Keystore & Building the AAB

### Step 1: Generate Release Keystore
```bash
keytool -genkey -v -keystore dreamtalk-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias dreamtalk -storetype JKS
```

### Step 2: Configure `android/key.properties`
Create `android/key.properties`:
```properties
storePassword=your_store_password
keyPassword=your_key_password
keyAlias=dreamtalk
storeFile=/path/to/dreamtalk-release-key.jks
```

### Step 3: Build Android App Bundle (AAB)
```bash
cd flutter_app
flutter clean
flutter pub get
flutter build appbundle --release
```

The output file will be at:
`build/app/outputs/bundle/release/app-release.aab`
Upload this `.aab` file directly to the Google Play Console Production or Closed Testing track!
