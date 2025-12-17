# Famly Website - Deep Linking Setup

Dit is de website voor famly.app, nodig voor deep linking functionaliteit.

## Setup Instructies

### 1. Apple Team ID vinden
1. Ga naar https://developer.apple.com/account
2. Klik op "Membership" in het menu
3. Je ziet je **Team ID** (bijv. `ABC1234567`)
4. Vervang `YOUR_TEAM_ID` in `public/.well-known/apple-app-site-association`

### 2. Android SHA256 Fingerprint ophalen

Voor je **release keystore**:
```bash
keytool -list -v -keystore android/upload-keystore.jks
```

Of via Play Console:
1. Ga naar Play Console → je app
2. Release → Setup → App signing
3. Kopieer de **SHA-256 certificate fingerprint**
4. Vervang `YOUR_SHA256_FINGERPRINT_HERE` in `public/.well-known/assetlinks.json`

### 3. Deploy naar Vercel

Zie DEPLOY_INSTRUCTIONS.md voor complete stappen.

## Bestanden

- `public/index.html` - Landing page voor famly.app
- `public/.well-known/apple-app-site-association` - iOS Universal Links config
- `public/.well-known/assetlinks.json` - Android App Links config
- `vercel.json` - Vercel configuratie voor correcte headers

## Testen

Na deployment:
```bash
# Test iOS config
curl https://famly.app/.well-known/apple-app-site-association

# Test Android config
curl https://famly.app/.well-known/assetlinks.json
```
