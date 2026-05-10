# GaitGuard CRM — Firebase Setup

The GaitGuard CRM works **offline-first** out of the box. All leads are stored in your browser's localStorage and the app works without any backend. Firebase is optional — it adds multi-device sync (so the same data shows up on your laptop and phone, and on any teammate's machine).

You only need this guide if you want sync.

## What you get with Firebase

- Same leads visible on every browser/device you sign into
- Real-time updates: change a stage on your phone, watch it move on your laptop
- A safety net if your laptop's localStorage gets cleared
- Future-proofing for when you hire an SDR or VA

## Setup (one-time, ~10 minutes)

1. Go to https://console.firebase.google.com and click **Add project**.
2. Name it `gaitguard-crm` (or whatever). Disable Google Analytics — you don't need it for an internal tool.
3. In your new project, click **Build → Firestore Database → Create database**. Pick the **eu-west** region (closest to UK) and start in **production mode**.
4. Open the **Rules** tab and paste this, then **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

   This requires anonymous-auth sign-in. The CRM signs in anonymously on load.

5. Click **Build → Authentication → Get started → Sign-in method**. Enable **Anonymous**.

6. Back on the project home, click the `</>` web icon to register a web app. Give it a nickname (e.g. `gaitguard-crm-web`). Skip Firebase Hosting unless you want it. Copy the config object — it looks like:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "gaitguard-crm.firebaseapp.com",
     projectId: "gaitguard-crm",
     storageBucket: "gaitguard-crm.firebasestorage.app",
     messagingSenderId: "...",
     appId: "1:..."
   };
   ```

7. Open `crm.html` in this repo, find the block near the top:

   ```js
   window.GAITGUARD_FIREBASE_CONFIG = {
     apiKey: "YOUR_API_KEY",
     ...
   };
   ```

   Replace each placeholder with the matching value from step 6. Save. Reload `crm.html` in your browser — you should see the sync indicator go green ("Connected. Data will sync across all devices.") in the top bar.

## Costs

For one founder + a few collaborators, you stay inside Firebase's free Spark plan indefinitely. Free tier covers ~50K reads/day and 1GB storage — way beyond what a CRM at GaitGuard's stage uses.

## Anything else?

- The CRM never crashes if Firebase fails. It silently falls back to localStorage-only mode.
- If you delete the Firebase config (revert to placeholders), the app reverts to local-only without losing your data.
- Multi-device tip: open the same CRM URL on every device and sign in as Melusi each time. The data is keyed to the Firebase project, not the device.
