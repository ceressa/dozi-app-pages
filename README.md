<p align="center">
  <img src="https://img.shields.io/badge/Dozi-Website-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white" alt="Dozi Website"/>
</p>

<h1 align="center">Dozi Website</h1>

<p align="center">
  <strong>🌐 Official website for Dozi - Smart Medication Reminder App</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5"/>
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3"/>
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript"/>
  <img src="https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black" alt="Firebase"/>
  <img src="https://img.shields.io/badge/GitHub%20Pages-222222?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Pages"/>
</p>

<p align="center">
  <a href="https://med.bardino.app">🌐 Live Website</a> • 
  <a href="https://med.bardino.app/en">🇬🇧 English</a> • 
  <a href="https://med.bardino.app/blog">📝 Blog</a>
</p>

---

## 🎯 Overview

Official marketing website for **Dozi**, the AI-powered medication reminder app. Built with vanilla HTML/CSS/JavaScript and hosted on GitHub Pages with custom domain.

---

## ✨ Features

### 🌍 Multilingual Support

| Language | URL | Status |
|----------|-----|--------|
| **Turkish** (Primary) | `https://med.bardino.app` | ✅ Live |
| **English** | `https://med.bardino.app/en` | ✅ Live |

### 📄 Pages

| Page | Turkish | English | Description |
|------|---------|---------|-------------|
| **Homepage** | `/index.html` | `/en/index.html` | Main landing page with features |
| **Download** | `/app/download.html` | `/app/index.html` | App download page |
| **Web Dashboard** | `/dozi/dashboard.html` | - | User medication dashboard (Firebase Auth) |
| **Blog** | `/blog/index.html` | - | Turkish blog articles |
| **Privacy Policy** | `/privacy-policy.html` | - | Privacy policy (KVKK/GDPR) |
| **Terms of Use** | `/terms-of-use.html` | - | Terms and conditions |
| **Cookie Policy** | `/cookie-policy.html` | - | Cookie usage policy |
| **Account Deletion** | `/account-deletion.html` | `/en/account-deletion.html` | Account deletion request form |
| **Pharmacies** | `/eczacilara.html` | - | Pharmacy partnership page |

### 🎯 Web Dashboard Features

**URL**: `https://med.bardino.app/dozi/dashboard.html`

The web dashboard provides users with a comprehensive view of their medication tracking:

- **📊 Genel Bakış (Overview)**: Stats cards, weekly chart, recent activity
- **💊 İlaçlarım (My Medicines)**: Beautiful medicine cards with dosage, frequency, and times
- **📅 Bugün (Today)**: Timeline view with Al/Atla/Ertele action buttons
- **👥 Badilerim (My Buddies)**: Family tracking contacts with avatar cards
- **📈 İstatistikler (Statistics)**: Monthly performance charts and adherence metrics

**Technical Stack**:
- Firebase Auth for authentication
- Firebase Functions for backend API (`getUserDashboardData`, `markMedicationTaken`)
- Chart.js for data visualization
- Responsive design with mobile support
- Real-time data sync with Firestore

**Security**: Top-level security with Firebase Auth session management and secure HTTPS.

### 📝 Blog Articles (Turkish)

1. **En İyi İlaç Hatırlatıcı 2026** - Comparison guide
2. **İlaç Hatırlatıcı Nasıl Seçilir** - Selection criteria
3. **İlaç Unutmak Neden Yaygın** - Why people forget medications
4. **Yaşlı Ebeveyn İlaç Takibi** - Elderly care guide
5. **Sabah Rutini İlaç Uyumu** - Morning routine tips
6. **İlaç Takip Seridi Motivasyon** - Streak motivation
7. **İlaç Stoku Takibi Neden Önemli** - Stock tracking importance
8. **Konum Bazlı Hatırlatıcı** - Location-based reminders
9. **AI Okunabilirlik Optimizasyonu** - AI readability optimization

### 🔧 Technical Features

- **Static Site**: Pure HTML/CSS/JavaScript (no build process)
- **GitHub Pages**: Hosted on GitHub Pages with custom domain
- **Custom Domain**: `med.bardino.app` with CNAME configuration
- **SSL/HTTPS**: Automatic HTTPS via GitHub Pages
- **SEO Optimized**: Meta tags, Open Graph, Twitter Cards
- **Responsive Design**: Mobile-first responsive layout
- **Analytics**: Firebase pageview tracking
- **Social Sharing**: Share buttons for social media
- **Sitemap**: XML sitemap for search engines
- **Robots.txt**: Search engine crawling configuration

### 📊 Analytics Integration

- **Firebase Cloud Function**: `trackPageView` for pageview tracking
- **Firestore Collection**: `website_pageviews` for analytics data
- **Tracked Data**:
  - Page URL and title
  - Referrer source
  - User language
  - Screen resolution
  - Timestamp and IP address
  - Page category (homepage, blog, legal, etc.)

### 🔐 Legal & Compliance

- **KVKK Compliance**: Turkish data protection law
- **GDPR Compliance**: EU data protection regulation
- **Cookie Policy**: Cookie usage disclosure
- **Privacy Policy**: Data collection and usage
- **Account Deletion**: Self-service account deletion form
- **KVKK Request Form**: `/legal/kvkk-basvuru.html`
- **GDPR Request Form**: `/legal/gdpr-request.html`

### 🏥 Pharmacy Partnerships

- **Partnership Page**: `/eczacilara.html`
- **Brochure Content**: `docs/03-features/PHARMACY_BROCHURE_CONTENT.md`
- **Outreach Strategy**: `docs/03-features/PHARMACY_OUTREACH_STRATEGY.md`
- **Email Templates**: `docs/03-features/PARTNERSHIP_EMAIL_TEMPLATES.md`

---

## 📁 Project Structure

```
dozi-website-temp/
├── index.html                      # Turkish homepage
├── en/
│   ├── index.html                  # English homepage
│   └── account-deletion.html       # English account deletion
├── dozi/
│   ├── dashboard.html              # Web dashboard (Firebase Auth)
│   ├── dashboard.js                # Dashboard logic and API calls
│   ├── dashboard.css               # Modern dashboard styling
│   ├── auth.js                     # Firebase authentication
│   ├── index.html                  # Dashboard login page
│   ├── images/                     # Dozi brand images
│   │   ├── dozi_brand.webp
│   │   ├── dozi_happy.webp
│   │   └── dozi_logo.webp
│   ├── README.md                   # Dashboard documentation
│   ├── SECURITY.md                 # Security implementation
│   └── DEPLOYMENT.md               # Deployment guide
├── app/
│   ├── download.html               # Turkish download page
│   └── index.html                  # English download page
├── blog/
│   ├── index.html                  # Blog index
│   ├── en-iyi-ilac-hatirlat-2026.html
│   ├── ilac-hatirlat-nasil-secilir.html
│   ├── ilac-unutmak-neden-yaygin.html
│   ├── yasli-ebeveyn-ilac-takibi.html
│   ├── sabah-rutini-ilac-uyumu.html
│   ├── ilac-takip-seridi-motivasyon.html
│   ├── ilac-stoku-takibi-neden-onemli.html
│   ├── konum-bazli-hatirlat-eve-gelince.html
│   └── ai-okunabilirlik-optimizasyonu.html
├── legal/
│   ├── kvkk-basvuru.html           # KVKK request form
│   └── gdpr-request.html           # GDPR request form
├── css/
│   ├── social-share.css            # Social sharing styles
│   └── ...
├── js/
│   ├── pageview-tracker.js         # Firebase analytics tracker
│   ├── social-share.js             # Social sharing functionality
│   └── ...
├── images/                         # Image assets
├── .well-known/
│   └── assetlinks.json             # Android App Links verification
├── privacy-policy.html             # Privacy policy
├── terms-of-use.html               # Terms of use
├── cookie-policy.html              # Cookie policy
├── account-deletion.html           # Account deletion (Turkish)
├── account-deletion-form.html      # Account deletion form
├── eczacilara.html                 # Pharmacy partnerships
├── sitemap.xml                     # XML sitemap
├── robots.txt                      # Robots configuration
├── CNAME                           # Custom domain configuration
├── _config.yml                     # Jekyll configuration
├── .nojekyll                       # Disable Jekyll processing
├── CHANGELOG.md                    # Version history
├── DEPLOYMENT_CHECKLIST.md         # Deployment guide
├── IMPLEMENTATION_SUMMARY.md       # Implementation notes
├── QUICK_REFERENCE.md              # Quick reference
├── SECURITY.md                     # Security documentation
└── README.md                       # This file
```

---

## 🚀 Deployment

### GitHub Pages Setup

1. **Repository Settings**
   - Go to repository Settings → Pages
   - Source: Deploy from `main` branch
   - Custom domain: `med.bardino.app`

2. **DNS Configuration**
   - Add CNAME record: `med.bardino.app` → `ceressa.github.io`
   - Add A records for apex domain:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```

3. **CNAME File**
   - Create `CNAME` file in root with content: `med.bardino.app`

4. **Deploy**
   ```bash
   git add .
   git commit -m "Update website"
   git push origin main
   ```

5. **Verify**
   - Wait 1-2 minutes for GitHub Pages to rebuild
   - Visit `https://med.bardino.app` to verify

See `DEPLOYMENT_CHECKLIST.md` for detailed deployment steps.

---

## 🛠 Development

### Local Development

1. **Use a local server** (required for proper testing)
   ```bash
   # Python 3
   python -m http.server 8000
   
   # Node.js (http-server)
   npx http-server -p 8000
   ```

2. **Access locally**
   ```
   http://localhost:8000
   ```

### Encoding Safety ⚠️

**CRITICAL**: Never use PowerShell or Python scripts to modify HTML files with Turkish characters!

- ❌ **NEVER**: `(Get-Content index.html) -replace 'old', 'new' | Set-Content index.html`
- ✅ **ALWAYS**: Use manual editing or Kiro's `fsWrite`/`strReplace` tools

Turkish characters (ı, ç, ü, ö, ğ, ş, İ, Ç, Ş, Ğ, Ü, Ö) and emojis can be corrupted by scripts.

See `.kiro/steering/encoding-safety.md` for details.

---

## 📊 Analytics

### Firebase Pageview Tracking

**Setup:**
1. Firebase Cloud Function: `trackPageView` (deployed)
2. JavaScript tracker: `js/pageview-tracker.js` (included on all pages)
3. Firestore collection: `website_pageviews`

**Tracked Data:**
- Page URL, title, category
- Referrer source
- User language
- Screen resolution
- Timestamp and IP
- Unique visitor identification

**Admin Panel:**
View analytics at `dozi-admin-panel` → Website Analytics page

---

## 🔗 Important Links

- **Live Website**: [med.bardino.app](https://med.bardino.app)
- **English Site**: [med.bardino.app/en](https://med.bardino.app/en)
- **Blog**: [med.bardino.app/blog](https://med.bardino.app/blog)
- **Download**: [med.bardino.app/app/download.html](https://med.bardino.app/app/download.html)
- **Google Play**: [Download Dozi](https://play.google.com/store/apps/details?id=com.bardino.dozi)

---

## 📞 Contact

- **Website:** [med.bardino.app](https://med.bardino.app)
- **Email:** [info@bardino.app](mailto:info@bardino.app)
- **Support:** [support@bardino.app](mailto:support@bardino.app)

---

## 📄 License

This website is proprietary. All rights reserved.

**© 2024-2026 Bardino. Dozi is a registered trademark.**

---

<p align="center">
  <strong>Made with ❤️ in Turkey 🇹🇷</strong>
</p>
