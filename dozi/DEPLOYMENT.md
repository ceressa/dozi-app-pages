# Dozi Web Dashboard - Deployment Guide

## 🚀 GitHub Pages Deployment

### 1. Firebase Functions Deploy

```bash
cd Dozi/firebase-functions
npm run deploy
```

Veya sadece web dashboard functions:
```bash
firebase deploy --only functions:verifyWebLogin,functions:getUserDashboardData,functions:markMedicationTaken,functions:getBadiDashboardData,functions:markBadiMedicationTaken
```

### 2. Firebase Config Güncelle

`dozi-website-temp/dozi/auth.js` ve `dashboard.js` dosyalarındaki Firebase config'i gerçek değerlerle değiştir:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_REAL_API_KEY",
    authDomain: "dozi-app.firebaseapp.com",
    projectId: "dozi-app",
    storageBucket: "dozi-app.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

### 3. GitHub Repository Setup

```bash
cd dozi-website-temp
git init
git add .
git commit -m "Initial commit: Dozi Web Dashboard"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/dozi-website.git
git push -u origin main
```

### 4. GitHub Pages Aktifleştir

1. GitHub repository → Settings → Pages
2. Source: Deploy from a branch
3. Branch: `main` / `root`
4. Save

Site URL: `https://YOUR_USERNAME.github.io/dozi-website/`

### 5. Custom Domain (med.bardino.app)

#### GitHub Pages Ayarları
1. Repository Settings → Pages → Custom domain
2. `med.bardino.app` yaz ve Save

#### DNS Ayarları (Domain Provider)
```
A Record:
@  →  185.199.108.153
@  →  185.199.109.153
@  →  185.199.110.153
@  →  185.199.111.153

CNAME Record:
www  →  YOUR_USERNAME.github.io
```

#### CNAME Dosyası
`dozi-website-temp/CNAME` dosyası oluştur:
```
med.bardino.app
```

### 6. HTTPS Aktifleştir

GitHub Pages → Settings → Enforce HTTPS ✅

## 🔐 Güvenlik Ayarları

### Firebase Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // User data - sadece kendi verilerine erişim
    match /users/{userId} {
      allow read: if request.auth != null && request.auth.uid == userId;
      allow write: if request.auth != null && request.auth.uid == userId;
      
      // Medicines subcollection
      match /medicines/{medicineId} {
        allow read: if request.auth != null && request.auth.uid == userId;
        allow write: if request.auth != null && request.auth.uid == userId;
      }
      
      // Badis subcollection
      match /badis/{badiId} {
        allow read: if request.auth != null && request.auth.uid == userId;
        allow write: if request.auth != null && request.auth.uid == userId;
      }
    }
    
    // Medication logs - sadece kendi loglarına erişim
    match /medication_logs/{logId} {
      allow read: if request.auth != null && 
                     resource.data.userId == request.auth.uid;
      allow write: if request.auth != null && 
                      request.resource.data.userId == request.auth.uid;
    }
  }
}
```

### Firebase Authentication

Google Sign-In aktif olmalı:
1. Firebase Console → Authentication → Sign-in method
2. Google → Enable
3. Authorized domains: `med.bardino.app`, `YOUR_USERNAME.github.io`

## 📁 Dosya Yapısı (GitHub)

```
dozi-website-temp/
├── CNAME                    # Custom domain
├── index.html              # Ana sayfa
├── dozi/
│   ├── index.html          # Login sayfası
│   ├── auth.js            # Auth logic
│   ├── dashboard.html     # Dashboard
│   ├── dashboard.css      # Styling
│   ├── dashboard.js       # Dashboard logic
│   ├── README.md          # Döküman
│   ├── SECURITY.md        # Güvenlik
│   └── DEPLOYMENT.md      # Bu dosya
├── blog/                   # Blog sayfaları
├── css/                    # Global CSS
├── js/                     # Global JS
└── images/                 # Görseller
```

## 🧪 Test

### Local Test
```bash
# Python HTTP server
cd dozi-website-temp
python -m http.server 8000

# Tarayıcıda aç
http://localhost:8000/dozi/
```

### Production Test
```bash
# GitHub Pages URL
https://YOUR_USERNAME.github.io/dozi-website/dozi/

# Custom domain
https://med.bardino.app/dozi/
```

## 🔄 Güncelleme

```bash
cd dozi-website-temp
git add .
git commit -m "Update: dashboard improvements"
git push origin main
```

GitHub Pages otomatik deploy eder (1-2 dakika).

## 📊 Monitoring

### Firebase Console
- Authentication → Users
- Firestore → Data
- Functions → Logs
- Analytics → Events

### GitHub Pages
- Repository → Actions (build logs)
- Settings → Pages (deployment status)

## 🐛 Troubleshooting

### "User not found" hatası
- Kullanıcı mobil uygulamada kayıtlı değil
- Firebase Functions `verifyWebLogin` loglarını kontrol et

### CORS hatası
- Firebase Functions region kontrolü (europe-west3)
- Authorized domains kontrolü

### 404 hatası
- GitHub Pages build tamamlanmadı (1-2 dakika bekle)
- CNAME dosyası doğru mu?
- DNS propagation (24-48 saat sürebilir)

### Firebase Functions timeout
- Functions region: europe-west3
- Firestore indexes oluşturuldu mu?

## 📞 Destek

- Firebase Console: https://console.firebase.google.com
- GitHub Pages Docs: https://docs.github.com/pages
- Dozi Docs: https://github.com/YOUR_USERNAME/dozi-website/wiki
