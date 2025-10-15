# Firebase Setup Guide

This guide will help you set up Firebase Cloud Firestore to store your tradeshow leads in the cloud.

## Benefits of Firebase Integration

- ✅ **Cloud Storage**: All leads saved to Firebase Firestore
- ✅ **Access Anywhere**: View leads from any device
- ✅ **Real-time Sync**: Data syncs automatically
- ✅ **Backup**: localStorage + Firebase for redundancy
- ✅ **Free Tier**: Up to 1GB storage and 50K reads/day

## Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"**
3. Enter project name: `cannabis-expo-leads` (or your choice)
4. Disable Google Analytics (optional, not needed)
5. Click **"Create project"**

## Step 2: Enable Firestore Database

1. In your Firebase project, click **"Firestore Database"** in the left sidebar
2. Click **"Create database"**
3. Select **"Start in production mode"** (we'll set rules next)
4. Choose your region (preferably closest to Buenos Aires, e.g., `southamerica-east1`)
5. Click **"Enable"**

## Step 3: Set Firestore Security Rules

1. In Firestore Database, click the **"Rules"** tab
2. Replace the rules with this:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /leads/{document=**} {
      allow read, write: if true;
    }
  }
}
```

3. Click **"Publish"**

⚠️ **Note**: These rules allow anyone to read/write. For production, you'd want authentication.

## Step 4: Get Your Firebase Config

1. Click the **gear icon** ⚙️ next to "Project Overview" → **"Project settings"**
2. Scroll down to **"Your apps"**
3. Click the **web icon** `</>`
4. Register app name: `cannabis-expo-leads`
5. **Don't check** "Also set up Firebase Hosting"
6. Click **"Register app"**
7. Copy the `firebaseConfig` object

## Step 5: Update index.html

1. Open `index.html` in your editor
2. Find this section (around line 267):

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

3. Replace it with YOUR Firebase config from Step 4
4. Save the file

## Step 6: Deploy to GitHub Pages

```bash
cd ~/Desktop/tradeshow-leads
git add index.html
git commit -m "Add Firebase configuration"
git push origin master
```

Wait 1-2 minutes for GitHub Pages to deploy.

## Step 7: Test It!

1. Open: https://designdream.github.io/cannabis-expo-leads/
2. Look for **"☁️ Cloud + Local: Listo"** indicator
3. Add a test lead
4. Check Firebase Console → Firestore Database → You should see the lead!

## Viewing Your Data

### In Firebase Console
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Click **"Firestore Database"**
4. Browse the `leads` collection

### Export from the App
- Click **"Exportar a CSV"** button to download all leads as a spreadsheet

## Troubleshooting

### "Storage: Ready" instead of "Cloud + Local"
- Check browser console (F12) for errors
- Verify Firebase config is correct
- Make sure Firestore is enabled in Firebase Console

### Leads not saving to Firebase
- Check Firestore Rules are published
- Check browser console for errors
- Verify you're on the deployed GitHub Pages URL (not local file)

### CORS Errors
- Must use HTTPS URL (GitHub Pages URL)
- Cannot test with `file://` protocol

## Security Note

The current setup allows anyone with the URL to read/write data. For a production app, you'd want to:
1. Add Firebase Authentication
2. Update Firestore rules to require authentication
3. Add user roles/permissions

For a tradeshow form, the current setup is acceptable since:
- The URL is not public
- Data isn't sensitive
- You'll export and delete after the event
