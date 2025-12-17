# Vercel Deployment Instructies

## Stap 1: Voorbereiden

### A. Apple Team ID invullen
1. Ga naar https://developer.apple.com/account
2. Klik op "Membership"
3. Kopieer je **Team ID** (bijv. `ABC1234567`)
4. Open `public/.well-known/apple-app-site-association`
5. Vervang `YOUR_TEAM_ID` met je echte Team ID

### B. Android SHA256 Fingerprint ophalen

**Optie 1: Via Play Console (Makkelijkst)**
1. Ga naar https://play.google.com/console
2. Selecteer je app "Famly"
3. Ga naar **Release → Setup → App integrity**
4. Onder "App signing" zie je de **SHA-256 certificate fingerprint**
5. Kopieer deze (een lange hexadecimale string)
6. Open `public/.well-known/assetlinks.json`
7. Vervang `YOUR_SHA256_FINGERPRINT_HERE` met deze fingerprint

**Optie 2: Via keystore bestand**
```bash
cd /Users/erven/famly_v2/android
keytool -list -v -keystore upload-keystore.jks
```
Zoek naar "SHA256:" en kopieer de waarde.

---

## Stap 2: GitHub Repository aanmaken

1. Ga naar https://github.com en log in
2. Klik op het **"+"** icoon rechtsboven → "New repository"
3. Naam: `famly-website`
4. Zet hem op **Public** (of Private, beide werken)
5. **NIET** "Initialize with README" aanvinken
6. Klik "Create repository"

### Repository koppelen aan je lokale bestanden:
```bash
cd /Users/erven/famly_v2/website
git init
git add .
git commit -m "Initial commit: Famly website with deep linking"
git branch -M main
git remote add origin https://github.com/JOUW_USERNAME/famly-website.git
git push -u origin main
```

Vervang `JOUW_USERNAME` met je GitHub username.

---

## Stap 3: Vercel Account aanmaken

1. Ga naar https://vercel.com
2. Klik op **"Sign Up"**
3. Kies **"Continue with GitHub"** (meest makkelijk)
4. Geef Vercel toegang tot je GitHub account

---

## Stap 4: Project deployen op Vercel

1. Klik op **"Add New..."** → **"Project"**
2. Je ziet nu je GitHub repositories
3. Zoek **"famly-website"** en klik **"Import"**
4. Bij "Configure Project":
   - **Framework Preset:** Select "Other" (of laat leeg)
   - **Root Directory:** `./` (standaard is goed)
   - **Build Command:** Laat leeg
   - **Output Directory:** `public`
5. Klik **"Deploy"**
6. Wacht 30-60 seconden... 🚀

Je krijgt nu een URL zoals: `famly-website-abc123.vercel.app`

---

## Stap 5: Custom Domain (famly.app) toevoegen

### A. Domain toevoegen in Vercel
1. Ga naar je project dashboard
2. Klik op **"Settings"** (bovenaan)
3. Klik op **"Domains"** in het menu
4. Klik **"Add"**
5. Vul in: `famly.app`
6. Klik **"Add"**

Vercel geeft je nu DNS instructies.

### B. DNS instellen bij je domain registrar

Je moet deze DNS records toevoegen waar je `famly.app` hebt geregistreerd (bijv. Namecheap, GoDaddy, etc.):

**Voor apex domain (famly.app):**
- Type: `A`
- Name: `@`
- Value: `76.76.21.21`

**Voor www subdomain (optioneel):**
- Type: `CNAME`
- Name: `www`
- Value: `cname.vercel-dns.com`

**DNS propagatie duurt 5 minuten tot 24 uur** ⏱️

### C. Verifiëren
Na DNS propagatie:
1. Ga terug naar Vercel → Domains
2. Je domain zou nu ✅ groen moeten zijn
3. Test in browser: https://famly.app

---

## Stap 6: Testen of alles werkt

### Test 1: Website bereikbaar
```bash
curl https://famly.app
```
Zou HTML moeten teruggeven.

### Test 2: iOS config
```bash
curl https://famly.app/.well-known/apple-app-site-association
```
Zou JSON moeten teruggeven met je Team ID.

### Test 3: Android config
```bash
curl https://famly.app/.well-known/assetlinks.json
```
Zou JSON moeten teruggeven met je SHA256 fingerprint.

### Test 4: Deep link
Test op je telefoon:
1. Stuur jezelf via WhatsApp: `https://famly.app/join?code=TEST123`
2. Tik op de link
3. Als de app geïnstalleerd is, zou deze moeten openen met de join dialog

---

## Veelgestelde vragen

**Q: Ik heb nog geen famly.app domain?**
A: Je moet dit domain eerst registreren bij een registrar zoals:
- Namecheap (namecheap.com)
- GoDaddy (godaddy.com)
- Cloudflare (cloudflare.com)

**Q: Kan ik eerst testen met de Vercel URL?**
A: Ja! Vervang in je Flutter app tijdelijk `famly.app` met je Vercel URL (bijv. `famly-website-abc123.vercel.app`). Dit werkt voor ontwikkeling.

**Q: Deep links werken niet direct?**
A: iOS en Android cachen Universal/App Links. Na deployment:
- iOS: Wacht 24 uur of verwijder + herinstalleer de app
- Android: Wis app data en herstart

**Q: Hoe update ik de bestanden?**
A: Bewerk bestanden lokaal, dan:
```bash
cd /Users/erven/famly_v2/website
git add .
git commit -m "Update beschrijving"
git push
```
Vercel deploy automatisch binnen 30 seconden!

---

## Checklist ✅

- [ ] Apple Team ID ingevuld in `apple-app-site-association`
- [ ] Android SHA256 ingevuld in `assetlinks.json`
- [ ] GitHub repository aangemaakt
- [ ] Code gepushed naar GitHub
- [ ] Vercel account aangemaakt
- [ ] Project gedeployed op Vercel
- [ ] Domain famly.app toegevoegd in Vercel
- [ ] DNS records ingesteld
- [ ] Website bereikbaar op https://famly.app
- [ ] Deep link configs bereikbaar
- [ ] Deep link getest op telefoon

---

## Support

Als je ergens vastloopt, check:
- Vercel docs: https://vercel.com/docs
- GitHub docs: https://docs.github.com
