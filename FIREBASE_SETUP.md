# Firebase Setup Instructions

To enable shared upvotes across all users, you need to set up a Firebase Realtime Database.

## Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project" or "Create a project"
3. Enter a project name (e.g., "wetlab-database")
4. Follow the setup wizard (you can disable Google Analytics if you don't need it)

## Step 2: Set Up Realtime Database

1. In your Firebase project, go to **Build > Realtime Database**
2. Click **Create Database**
3. Choose a location (e.g., United States)
4. Start in **Test mode** (we'll set up rules next)
5. Click **Enable**

## Step 3: Configure Database Rules

1. Go to the **Rules** tab in Realtime Database
2. Replace the rules with this:

```json
{
  "rules": {
    "upvotes": {
      ".read": true,
      ".write": true,
      "$entryId": {
        ".validate": "newData.isNumber() && newData.val() >= 0"
      }
    },
    "comments": {
      ".read": true,
      ".write": true
    }
  }
}
```

3. Click **Publish**

These rules allow:
- Anyone to read upvote counts
- Anyone to write upvote counts
- Values must be non-negative numbers
- Only the `upvotes` path is accessible

## Step 4: Get Your Firebase Config

1. Go to **Project Settings** (gear icon in sidebar)
2. Scroll down to **Your apps**
3. Click the **Web** icon (`</>`)
4. Register your app (you can name it "Wetlab Web App")
5. Copy the `firebaseConfig` object

It will look something like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyAbc123...",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.firebaseio.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

## Step 5: Update index.html

1. Open `index.html`
2. Find the `firebaseConfig` object (around line 455)
3. Replace the dummy config with your actual config from Step 4
4. Save the file

## Step 6: Test It

1. Open `index.html` in your browser
2. Open the browser console (F12)
3. You should see "Firebase initialized successfully"
4. Click some upvote buttons
5. Open the page in another browser or incognito window
6. You should see the same upvote counts!

## Troubleshooting

**"Firebase initialization failed"**
- Check that you copied the config correctly
- Make sure the databaseURL is included
- Verify your Firebase project is active

**Upvotes not syncing**
- Check your database rules are set correctly
- Look at the Realtime Database tab in Firebase Console to see if data is being written
- Check browser console for errors

**Security concerns**
- The current rules allow anyone to modify upvotes
- For production, consider adding rate limiting or authentication
- You can also add validation rules to prevent abuse

## Fallback Mode

If Firebase fails to initialize, the app automatically falls back to localStorage-only mode. This means upvotes will only be stored locally on each user's device, but the app will still function.
