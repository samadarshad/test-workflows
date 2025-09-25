# App-Specific Connected App IDs Configuration

This guide explains how to configure app-specific `BITRISE_CONNECTED_APP_ID` values for each of your apps' Android builds using GitHub repository variables.

## Why This Is Needed

To provide **public download links** for Android APKs, we need to use Bitrise's Release Management API instead of the standard Builds API:

- **Standard Builds API**: ❌ No public download links (requires authentication)
- **Release Management API**: ✅ Provides public install page URLs for end users

Each app requires its unique **Connected App ID** to query the Release Management API. Without these configured:
- ❌ No public download links in Slack notifications
- ✅ Builds still complete successfully

## Setting Up App-Specific Connected App IDs

### Step 1: Get Your Connected App IDs from Bitrise

1. **Go to Bitrise Release Management**
   - Visit: https://app.bitrise.io/release-management/workspaces/9ede7cc2bd8c3a15/connected-apps
   - Select your organization

2. **For each app, find the Connected App ID:**
   - Click on the app in Release Management
   - Look at the URL: `https://app.bitrise.io/release-management/workspaces/9ede7cc2bd8c3a15/connected-apps/[CONNECTED_APP_ID]/build-distributions`
   - Copy the `CONNECTED_APP_ID` from the URL (the UUID after `/connected-apps/`)

3. **Repeat for all your apps:**
   - ev.energy → Copy its connected app ID
   - NGMA → Copy its connected app ID  
   - NGNY → Copy its connected app ID
   - SVCE → Copy its connected app ID
   - MCE → Copy its connected app ID
   - ANWB → Copy its connected app ID

### Step 2: Create Repository Variables

1. **Navigate to Repository Settings**
   - Go to your repository on GitHub
   - Click **Settings** tab
   - In left sidebar: **Secrets and variables** → **Actions**
   - Click **Variables** tab

2. **Create variables for each app:**
   - Click **New repository variable**
   - Create these variables with their respective connected app IDs:

| Variable Name | Description | Example Value |
|---------------|-------------|---------------|
| `BITRISE_CONNECTED_APP_ID_EV_ENERGY` | Connected app ID for ev.energy | `abc123def456...` |
| `BITRISE_CONNECTED_APP_ID_NGMA` | Connected app ID for NGMA | `def456ghi789...` |
| `BITRISE_CONNECTED_APP_ID_NGNY` | Connected app ID for NGNY | `ghi789jkl012...` |
| `BITRISE_CONNECTED_APP_ID_SVCE` | Connected app ID for SVCE | `jkl012mno345...` |
| `BITRISE_CONNECTED_APP_ID_MCE` | Connected app ID for MCE | `mno345pqr678...` |
| `BITRISE_CONNECTED_APP_ID_ANWB` | Connected app ID for ANWB | `pqr678stu901...` |

### Step 3: Verify Configuration

Ensure all your apps have their corresponding variables configured:
- Each app must have its own `BITRISE_CONNECTED_APP_ID_{APP_NAME}` variable
- No fallback mechanism exists - each app requires its specific connected app ID
- Missing variables will cause the workflow to skip Release Management functionality for that app

## Testing Your Configuration

1. **Run the Build Apps workflow**
2. **Check the logs** for each Android build
3. **Look for the "Resolve connected app ID" step**
4. **Verify the correct ID is being used** for each app

## Troubleshooting

### Problem: "Connected app ID not available"
**Solution:** 
- Check variable name matches the pattern exactly
- Ensure the variable value is the correct connected app ID
- Verify you're on the Variables tab (not Secrets tab)
- Each app must have its own specific variable configured

### Problem: "Install page URL not found"
**Possible causes:**
- Wrong connected app ID for the app
- Build hasn't appeared in Release Management yet (wait a few minutes)
- Connected app not properly configured in Bitrise
- App-specific variable not configured correctly 

## Security Notes
- **Keep access tokens as secrets** - Never put `BITRISE_ACCESS_TOKEN` in variables

---

*For questions about connected app IDs, check the Bitrise Release Management documentation or contact your Bitrise admin.*