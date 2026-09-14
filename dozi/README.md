# Dozi Web Dashboard

Modern, güvenli ve kullanıcı dostu web tabanlı ilaç takip dashboard'u.

## 🎯 Mimari

- **Frontend**: GitHub Pages (Static HTML/CSS/JS)
- **Backend**: Firebase Functions (Serverless)
- **Auth**: Firebase Authentication (Google Sign-In)
- **Database**: Firestore (via Functions)
- **Hosting**: GitHub Pages + Custom Domain (med.bardino.app)

## 🚀 Deployment

```bash
# 1. Firebase Functions deploy
cd Dozi/firebase-functions
firebase deploy --only functions:verifyWebLogin,functions:getUserDashboardData,functions:markMedicationTaken,functions:getBadiDashboardData,functions:markBadiMedicationTaken

# 2. GitHub Pages push
cd dozi-website-temp
git add .
git commit -m "Deploy web dashboard"
git push origin main
```

Detaylı bilgi: [DEPLOYMENT.md](./DEPLOYMENT.md)

## 🎨 Özellikler

### Kullanıcı Özellikleri
- ✅ **Google ile Güvenli Giriş** - Firebase Authentication
- 💊 **İlaç Yönetimi** - Tüm ilaçlarınızı görüntüleyin
- 📅 **Bugünkü Dozlar** - Günlük ilaç takibi ve "Aldım" işaretleme
- 👥 **Badi Takibi** - Sevdiklerinizin ilaç durumunu görün
- 📊 **İstatistikler** - Uyum oranı, seri, grafikler
- 🔄 **Real-time Sync** - Firebase Functions ile güvenli veri erişimi

### Teknik Özellikler
- 🔐 **Üst Düzey Güvenlik** - Firebase Functions, HTTPS only
- 🎨 **Modern UI/UX** - Glassmorphism, smooth animations
- 📱 **Responsive Design** - Mobil, tablet, desktop
- 📈 **Chart.js Entegrasyonu** - Canlı grafikler
- ⚡ **Hızlı Yükleme** - Optimize edilmiş performans
- 🌙 **Dark Mode** - Göz dostu karanlık tema

## 📁 Dosya Yapısı

```
dozi/
├── index.html          # Login sayfası
├── auth.js            # Authentication (Firebase Functions)
├── dashboard.html     # Ana dashboard
├── dashboard.css      # Modern styling
├── dashboard.js       # Dashboard logic (Firebase Functions)
├── README.md          # Bu dosya
├── SECURITY.md        # Güvenlik dökümanı
└── DEPLOYMENT.md      # Deployment rehberi
```

## 🔐 Güvenlik

### Backend (Firebase Functions)
- ✅ Authentication kontrolü (her function call)
- ✅ Rate limiting (abuse prevention)
- ✅ Kullanıcı doğrulama (sadece Dozi app kullanıcıları)
- ✅ Rol bazlı erişim (badi permissions)
- ✅ Input validation
- ✅ Error handling

### Frontend
- ✅ HTTPS only (GitHub Pages)
- ✅ XSS protection
- ✅ CSRF protection
- ✅ Clickjacking prevention
- ✅ Session management

Detaylı bilgi: [SECURITY.md](./SECURITY.md)

## 🔧 Firebase Functions

### verifyWebLogin
Kullanıcı girişini doğrular ve web login activity loglar.

```javascript
const verifyUser = functions.httpsCallable('verifyWebLogin');
const response = await verifyUser({ uid, email });
```

### getUserDashboardData
Kullanıcının tüm dashboard verilerini getirir (medicines, badis, logs).

```javascript
const getDashboardData = functions.httpsCallable('getUserDashboardData');
const response = await getDashboardData();
```

### markMedicationTaken
İlacı "alındı" olarak işaretler.

```javascript
const markTaken = functions.httpsCallable('markMedicationTaken');
const response = await markTaken({ logId });
```

### getBadiDashboardData
Badi'nin ilaç verilerini getirir (permission kontrolü ile).

```javascript
const getBadiData = functions.httpsCallable('getBadiDashboardData');
const response = await getBadiData({ badiUserId });
```

### markBadiMedicationTaken
Badi adına ilacı işaretler (permission kontrolü ile).

```javascript
const markBadiTaken = functions.httpsCallable('markBadiMedicationTaken');
const response = await markBadiTaken({ logId, badiUserId });
```

## 📊 Veri Akışı

```
Frontend (GitHub Pages)
    ↓ Firebase Auth
Firebase Authentication
    ↓ ID Token
Firebase Functions (Backend)
    ↓ Firestore Query
Firestore Database
    ↓ Data
Firebase Functions
    ↓ Response
Frontend (Dashboard)
```

## 🎨 Tasarım Sistemi

### Renkler
```css
--primary: #6366f1      /* Ana renk */
--secondary: #8b5cf6    /* İkincil renk */
--success: #10b981      /* Başarı */
--danger: #ef4444       /* Hata */
--warning: #f59e0b      /* Uyarı */
```

### Tipografi
- Font: System fonts (-apple-system, Segoe UI, Roboto)
- Başlıklar: 700 weight
- Body: 400-500 weight

### Animasyonlar
- Smooth transitions (0.3s ease)
- Hover effects
- Loading states
- Page transitions

## 🔮 Gelecek Özellikler

- [ ] Push notifications (web)
- [ ] Offline mode (Service Worker)
- [ ] Export data (PDF, CSV)
- [ ] Multi-language support
- [ ] Dark/Light theme toggle
- [ ] Advanced analytics
- [ ] Badi messaging
- [ ] Medicine search
- [ ] Reminder customization
- [ ] Badi "Aldım/Atladım" butonu

## 📞 Destek

- Website: https://med.bardino.app
- Email: info@bardino.app
- GitHub: https://github.com/YOUR_USERNAME/dozi-website

## 📄 Lisans

© 2026 Dozi. Tüm hakları saklıdır.

