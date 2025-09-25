# App Configuration System

## 📋 bitrise-apps.json

The `bitrise-apps.json` file contains Bitrise app configurations:

### 🏗️ Structure

```json
{
  "app-name": {
    "bitrise_connected_app_id": "your-app-connected-app-id-here",
    "workflow_prefix": "workflow-prefix"
  }
}
```

## ➕ Adding a New App

### Step 1: Update bitrise-apps.json

Add a new app entry:

```json
"NewApp": {
  "bitrise_connected_app_id": "your-newapp-connected-app-id-here",
  "workflow_prefix": "newapp"
}
```

### Step 2: Configure Connected App ID

Replace `"your-newapp-connected-app-id-here"` with the actual Bitrise connected app ID:

1. **Go to Bitrise Release Management**
   - Visit: https://app.bitrise.io/release-management/workspaces/9ede7cc2bd8c3a15/connected-apps

2. **For each app, find the Connected App ID:**
   - Click on the app in Release Management
   - Look at the URL: `https://app.bitrise.io/release-management/workspaces/9ede7cc2bd8c3a15/connected-apps/[CONNECTED_APP_ID]/build-distributions`
   - Copy the `CONNECTED_APP_ID` from the URL (the UUID after `/connected-apps/`)

3. **Find the Workflow Prefix:**
   - Go to your Bitrise project's Workflow Editor
   - Example: https://app.bitrise.io/app/3149db5a-b386-4ebe-8d9e-60eb5db82f03/workflow_editor#!/workflows?workflow_id=anwb-android-staging
   - Look at the workflow names (e.g., `anwb-android-staging`, `anwb-ios-production`)
   - The prefix is the first part before the platform: `anwb`

4. **Update the JSON configuration:**
   - Replace placeholders with actual values

**Example:**
```json
"bitrise_connected_app_id": "12345abc-6789-def0-1234-56789abcdef0",
"workflow_prefix": "anwb"
```

### Step 3: That's It! 🎉

The app will automatically be included when "All Apps" is selected in the workflow.

**Optionally**: Add "NewApp" to the workflow dispatch input dropdown for individual app selection.