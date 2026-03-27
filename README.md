# JobScout

A mobile-first PWA for evaluating job listings against your personal priorities and experience. Evaluations are scored by Claude AI across four dimensions and stored in your OneDrive.

## Setup

### 1. Azure App Registration (one-time)

1. Go to [portal.azure.com](https://portal.azure.com) and sign in with your Microsoft account
2. Navigate to **Azure Active Directory → App Registrations → New Registration**
3. Name: `JobScout`
4. Supported account types: **Personal Microsoft accounts only**
5. Platform: **Single-page application (SPA)**
6. Redirect URIs:
   - `https://YOUR-USERNAME.github.io/job-scout/` (for production)
   - `http://localhost:5500/` or whatever port you use locally
7. After registering, go to **API Permissions** and confirm these are present (they should be by default or easy to add):
   - `User.Read`
   - `Files.ReadWrite`
   - `Mail.Send`
8. Copy your **Application (client) ID**

### 2. Configure the app

Open `index.html` and find this line near the top of the `<script>`:

```javascript
const MSAL_CLIENT_ID = 'YOUR_AZURE_APP_CLIENT_ID';
```

Replace `YOUR_AZURE_APP_CLIENT_ID` with the Client ID you copied.

### 3. Add PWA icons

Add two PNG icon files to the project root:
- `icon-192.png` — 192×192px
- `icon-512.png` — 512×512px

These are required for the "Add to Home Screen" prompt to work properly. A simple solid blue square with "JS" in white works fine.

### 4. Deploy to GitHub Pages

1. Push the project to a GitHub repository named `job-scout`
2. Go to **Settings → Pages → Source: Deploy from branch → main / root**
3. Your app will be available at `https://YOUR-USERNAME.github.io/job-scout/`

### 5. First launch

1. Open the app and sign in with Microsoft
2. Navigate to **Settings**
3. Enter your **Anthropic API key** (from [console.anthropic.com](https://console.anthropic.com))
4. Enter your **email address** for digest emails
5. Tap **Save Settings** — this stores your settings in `JobScout/settings.json` in your OneDrive, making them available across all your devices

---

## File structure

```
job-scout/
  index.html         ← entire app (HTML + CSS + JS)
  manifest.json      ← PWA manifest
  service-worker.js  ← offline support
  icon-192.png       ← add manually
  icon-512.png       ← add manually
  README.md
```

## OneDrive storage

All data is stored in your personal OneDrive under `JobScout/`:

- `evaluations.json` — all saved evaluations
- `settings.json` — API key and email (synced across devices)

## Notes

- **Multiple devices**: sign in on any device and your evaluations and settings load automatically from OneDrive
- **Duplicate evaluations**: the same job listing can be evaluated multiple times; each is stored independently with its own timestamp
- **Culture Fit score**: based on Claude's training knowledge — confidence level is noted in each summary
- **CORS / URL fetching**: most job boards block client-side fetches; paste the description directly instead of relying on URL fetching (the URL field is for logging/reference only)
