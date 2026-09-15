# Dozi Web Dashboard - Güvenlik Dökümanı

## 🔐 Güvenlik Önlemleri

### 1. Kimlik Doğrulama (Authentication)

#### Firebase Authentication
```javascript
// Google Sign-In with popup
const provider = new firebase.auth.GoogleAuthProvider();
provider.setCustomParameters({
    prompt: 'select_account'  // Her seferinde hesap seçimi
});

await auth.signInWithPopup(provider);
```

**Güvenlik Özellikleri:**
- ✅ OAuth 2.0 protokolü
- ✅ Secure token management
- ✅ Auto token refresh
- ✅ Session persistence
- ✅ Multi-factor authentication ready

#### Kullanıcı Doğrulama
```javascript
// Sadece Dozi uygulamasında kayıtlı kullanıcılar
const userDoc = await db.collection('users').doc(user.uid).get();
if (!userDoc.exists) {
    throw new Error('Bu hesap Dozi uygulamasında kayıtlı değil.');
}
```

### 2. Veri Güvenliği (Data Security)

#### Firestore Security Rules

**Temel Prensipler:**
1. Varsayılan olarak her şey kapalı
2. Sadece authenticated kullanıcılar erişebilir
3. Kullanıcılar sadece kendi verilerine erişebilir
4. Write işlemleri validation içerir

**Rules Örneği:**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Helper functions
    function isAuthenticated() {
      return request.auth != null;
    }
    
    function isOwner(userId) {
      return request.auth.uid == userId;
    }
    
    function isValidMedicine() {
      return request.resource.data.keys().hasAll(['name', 'isActive']) &&
             request.resource.data.name is string &&
             request.resource.data.name.size() > 0 &&
             request.resource.data.isActive is bool;
    }
    
    // User documents
    match /users/{userId} {
      allow read: if isAuthenticated() && isOwner(userId);
      allow write: if isAuthenticated() && isOwner(userId);
      
      // Medicines subcollection
      match /medicines/{medicineId} {
        allow read: if isAuthenticated() && isOwner(userId);
        allow create: if isAuthenticated() && isOwner(userId) && isValidMedicine();
        allow update: if isAuthenticated() && isOwner(userId) && isValidMedicine();
        allow delete: if isAuthenticated() && isOwner(userId);
      }
      
      // Badis subcollection
      match /badis/{badiId} {
        allow read: if isAuthenticated() && isOwner(userId);
        allow write: if isAuthenticated() && isOwner(userId);
      }
      
      // Reminder logs subcollection
      match /reminderLogs/{logId} {
        allow read: if isAuthenticated() && isOwner(userId);
        allow write: if isAuthenticated() && isOwner(userId);
      }
    }
    
    // Medication logs
    match /medication_logs/{logId} {
      allow read: if isAuthenticated() && 
                     resource.data.userId == request.auth.uid;
      allow create: if isAuthenticated() && 
                       request.resource.data.userId == request.auth.uid &&
                       request.resource.data.keys().hasAll(['userId', 'medicineId', 'scheduledTime', 'status']);
      allow update: if isAuthenticated() && 
                       resource.data.userId == request.auth.uid &&
                       request.resource.data.userId == request.auth.uid;
      allow delete: if isAuthenticated() && 
                       resource.data.userId == request.auth.uid;
    }
    
    // Rate limiting - max 100 reads per minute
    match /{document=**} {
      allow read: if isAuthenticated() && 
                     request.time < resource.data.lastRead + duration.value(1, 'm') ||
                     !('lastRead' in resource.data);
    }
  }
}
```

### 3. Network Güvenliği

#### HTTPS Only
```javascript
// Firebase Hosting otomatik HTTPS sağlar
// HTTP istekleri otomatik HTTPS'e yönlendirilir
```

#### Content Security Policy (CSP)
```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               script-src 'self' https://www.gstatic.com https://cdn.jsdelivr.net; 
               style-src 'self' 'unsafe-inline'; 
               img-src 'self' data: https:; 
               connect-src 'self' https://*.firebaseio.com https://*.googleapis.com;">
```

#### CORS Configuration
```javascript
// Firebase Hosting otomatik CORS yönetimi
// Sadece dozi.app domain'inden isteklere izin
```

### 4. XSS (Cross-Site Scripting) Koruması

#### Input Sanitization
```javascript
// Kullanıcı inputlarını sanitize et
function sanitizeInput(input) {
    const div = document.createElement('div');
    div.textContent = input;
    return div.innerHTML;
}

// Kullanım
const safeName = sanitizeInput(userInput);
```

#### Output Encoding
```javascript
// innerHTML yerine textContent kullan
element.textContent = userData.name;  // ✅ Güvenli
element.innerHTML = userData.name;    // ❌ Tehlikeli
```

### 5. CSRF (Cross-Site Request Forgery) Koruması

#### Firebase Token Verification
```javascript
// Her istekte Firebase ID token gönder
const idToken = await firebase.auth().currentUser.getIdToken();

fetch('/api/endpoint', {
    headers: {
        'Authorization': `Bearer ${idToken}`
    }
});
```

### 6. Clickjacking Koruması

#### Frame Busting
```javascript
// Sayfanın iframe içinde açılmasını engelle
if (window.top !== window.self) {
    window.top.location = window.self.location;
}
```

#### X-Frame-Options Header
```json
// firebase.json
{
  "hosting": {
    "headers": [{
      "source": "**",
      "headers": [{
        "key": "X-Frame-Options",
        "value": "DENY"
      }]
    }]
  }
}
```

### 7. Session Management

#### Auto Logout
```javascript
// 30 dakika inaktivite sonrası otomatik çıkış
let inactivityTimer;

function resetInactivityTimer() {
    clearTimeout(inactivityTimer);
    inactivityTimer = setTimeout(() => {
        firebase.auth().signOut();
        window.location.href = 'index.html';
    }, 30 * 60 * 1000); // 30 minutes
}

// Event listeners
document.addEventListener('mousemove', resetInactivityTimer);
document.addEventListener('keypress', resetInactivityTimer);
```

#### Token Refresh
```javascript
// Firebase otomatik token refresh yapar
// Manuel refresh gerekirse:
const user = firebase.auth().currentUser;
const token = await user.getIdToken(true); // force refresh
```

### 8. Data Validation

#### Client-Side Validation
```javascript
function validateMedicineData(data) {
    if (!data.name || data.name.trim().length === 0) {
        throw new Error('İlaç adı gerekli');
    }
    
    if (data.name.length > 100) {
        throw new Error('İlaç adı çok uzun');
    }
    
    if (data.dosage && data.dosage.length > 50) {
        throw new Error('Doz bilgisi çok uzun');
    }
    
    if (data.stock && (data.stock < 0 || data.stock > 9999)) {
        throw new Error('Geçersiz stok miktarı');
    }
    
    return true;
}
```

#### Server-Side Validation (Firestore Rules)
```javascript
// Firestore rules ile server-side validation
function isValidMedicineData() {
    return request.resource.data.name is string &&
           request.resource.data.name.size() > 0 &&
           request.resource.data.name.size() <= 100 &&
           (!('dosage' in request.resource.data) || 
            request.resource.data.dosage.size() <= 50) &&
           (!('stock' in request.resource.data) || 
            (request.resource.data.stock >= 0 && 
             request.resource.data.stock <= 9999));
}
```

### 9. Rate Limiting

#### Firestore Query Limits
```javascript
// Her query'de limit kullan
const snapshot = await db.collection('medication_logs')
    .where('userId', '==', userId)
    .limit(500)  // Max 500 docs
    .get();
```

#### Request Throttling
```javascript
// Debounce kullanarak aşırı istek engelleme
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// Kullanım
const debouncedSearch = debounce(searchMedicines, 500);
```

### 10. Error Handling

#### Güvenli Error Messages
```javascript
// Kullanıcıya detaylı hata mesajı gösterme
try {
    await riskyOperation();
} catch (error) {
    console.error('Detailed error:', error);  // Log'a yaz
    showUserError('Bir hata oluştu. Lütfen tekrar deneyin.');  // Kullanıcıya genel mesaj
}
```

#### Error Logging
```javascript
// Firebase Analytics ile error tracking
firebase.analytics().logEvent('error', {
    error_type: error.code,
    error_message: error.message,
    user_id: currentUser.uid
});
```

### 11. Sensitive Data Protection

#### No Sensitive Data in Client
```javascript
// ❌ YAPMA: API keys, secrets client-side'da tutma
const API_KEY = 'secret_key_123';  // Tehlikeli!

// ✅ YAP: Firebase config public olabilir (domain restriction ile korunur)
const firebaseConfig = {
    apiKey: "AIzaSy...",  // Public, domain restriction ile güvenli
    authDomain: "dozi-app.firebaseapp.com"
};
```

#### Clear Sensitive Data
```javascript
// Sayfa kapatılırken sensitive data temizle
window.addEventListener('beforeunload', () => {
    // Clear any sensitive data from memory
    currentUser = null;
    userData = null;
    medicines = [];
    medicationLogs = [];
    console.clear();
});
```

### 12. Audit Logging

#### User Activity Logging
```javascript
// Önemli işlemleri logla
async function logUserActivity(action, details) {
    await db.collection('audit_logs').add({
        userId: currentUser.uid,
        action: action,
        details: details,
        timestamp: firebase.firestore.FieldValue.serverTimestamp(),
        ipAddress: await getUserIP(),
        userAgent: navigator.userAgent
    });
}

// Kullanım
await logUserActivity('MEDICINE_UPDATED', { medicineId: 'abc123' });
await logUserActivity('WEB_LOGIN', { email: user.email });
```

## 🚨 Güvenlik Kontrol Listesi

### Deployment Öncesi
- [ ] Firebase Security Rules test edildi
- [ ] HTTPS aktif
- [ ] CSP headers ayarlandı
- [ ] XSS koruması test edildi
- [ ] CSRF koruması aktif
- [ ] Clickjacking koruması aktif
- [ ] Rate limiting ayarlandı
- [ ] Error handling güvenli
- [ ] Sensitive data temizlendi
- [ ] Audit logging aktif

### Düzenli Kontroller
- [ ] Security rules güncel
- [ ] Dependencies güncel (npm audit)
- [ ] Firebase SDK güncel
- [ ] SSL sertifikası geçerli
- [ ] Audit logs incelendi
- [ ] Anormal aktivite kontrolü

## 📞 Güvenlik Sorunları

Güvenlik açığı bulursanız:
- Email: security@bardino.app
- Responsible disclosure policy
- Bug bounty program (yakında)

## 📚 Kaynaklar

- [Firebase Security Rules](https://firebase.google.com/docs/rules)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Web Security Best Practices](https://web.dev/secure/)
- [Firebase Security Checklist](https://firebase.google.com/support/guides/security-checklist)
