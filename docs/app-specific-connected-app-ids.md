# App-Specific Connected App IDs Configuration

This guide explains how to configure app-specific `BITRISE_CONNECTED_APP_ID` values for each of your apps' Android builds using GitHub repository variables.

## Why App-Specific Connected App IDs?

Each app in your Bitrise setup may have its own connected app in the Release Management system. This allows you to:

- 🎯 **Precise tracking** - Each app's builds are tracked separately in Release Management
- 🔒 **Better isolation** - Apps don't interfere with each other's install page generation
- 📊 **Cleaner analytics** - Separate analytics and download tracking per app
- 🚀 **Faster resolution** - Install page URLs resolve faster with app-specific IDs

## Setting Up App-Specific Connected App IDs

### Step 1: Get Your Connected App IDs from Bitrise

1. **Go to Bitrise Release Management**
   - Visit: https://app.bitrise.io/release-management
   - Select your organization

2. **For each app, find the Connected App ID:**
   - Click on the app in Release Management
   - Look at the URL: `https://app.bitrise.io/release-management/workspaces/[WORKSPACE_ID]/connected-apps/[CONNECTED_APP_ID]/build-distributions`
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

## How the Resolution Works

The workflow uses this logic for Android builds:

1. **Convert app name to variable format:**
   - `ev.energy` → `EV_ENERGY`
   - `NGMA` → `NGMA`
   - etc.

2. **Look for app-specific variable:**
   - Check for `BITRISE_CONNECTED_APP_ID_{APP_NAME}`
   - If found, use that connected app ID

3. **Skip if not found:**
   - Log warning message indicating which variable needs to be set
   - Skip Release Management API calls
   - Build succeeds but no install page URL

## Variable Naming Convention

Apps names are converted to variable names using this pattern:

| App Name | Variable Name |
|----------|---------------|
| `ev.energy` | `BITRISE_CONNECTED_APP_ID_EV_ENERGY` |
| `NGMA` | `BITRISE_CONNECTED_APP_ID_NGMA` |
| `NGNY` | `BITRISE_CONNECTED_APP_ID_NGNY` |
| `SVCE` | `BITRISE_CONNECTED_APP_ID_SVCE` |
| `MCE` | `BITRISE_CONNECTED_APP_ID_MCE` |
| `ANWB` | `BITRISE_CONNECTED_APP_ID_ANWB` |

**Rules:**
- Convert to uppercase
- Replace dots (`.`) with underscores (`_`)
- Prefix with `BITRISE_CONNECTED_APP_ID_`

## Workflow Output Examples

### ✅ With App-Specific Variable:
```
🔍 Resolving connected app ID for app: ev.energy
Looking for variable: BITRISE_CONNECTED_APP_ID_EV_ENERGY
✅ Using connected app ID: abc123de... (for ev.energy)
```

### ❌ No Connected App ID:
```
🔍 Resolving connected app ID for app: TestApp
Looking for variable: BITRISE_CONNECTED_APP_ID_TESTAPP
⚠️ Connected app ID not available - Release Management API unavailable
💡 Set BITRISE_CONNECTED_APP_ID_TESTAPP repository variable
```

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

## Benefits

✅ **Precise install page URLs** - Each app gets its correct install page  
✅ **Faster resolution** - No searching across wrong connected apps  
✅ **Better organization** - Clean separation between apps in Release Management  
✅ **Easier debugging** - Clear logs showing which ID is used for each app  
✅ **Flexible configuration** - Easy to update via GitHub UI without code changes  

## Security Notes

- **Variables are not encrypted** (unlike secrets)
- **Connected app IDs are not sensitive** - they're just identifiers
- **Keep access tokens as secrets** - Never put `BITRISE_ACCESS_TOKEN` in variables
- **Variables are visible** to anyone with repository access

---

*For questions about connected app IDs, check the Bitrise Release Management documentation or contact your Bitrise admin.*