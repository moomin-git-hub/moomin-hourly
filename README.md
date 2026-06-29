# Vaqt Banki · 2026

> Har bir daqiqa — bir tanga. Bir kun = 1440 tanga.

Vaqtni pul kabi boshqaradigan shaxsiy panel — bitta `index.html` fayl. Server yoki "build" shart emas. Ixtiyoriy **bulutli sinxron** bilan: telefon va kompyuterda bir akkaunt bilan bir xil ma'lumotni ko'rasiz.

Firebase sozlanmaган bo'lsa — ilova baribir to'liq ishlaydi, faqat **lokal rejim**da (ma'lumot shu brauzerda).

---

## Bulutli sinxronni ulash (ilova ichidan — fayl tahrirlamasdan)

Sinxron **Email/Parol** bilan ishlaydi (Google shart emas — shuning uchun "Authorized domains" muammosi yo'q) va **Realtime Database**'dan foydalanadi.

1. Ilovani oching → **Sozlamalar** bo'limi → **«☁ Bulutli sinxron (Firebase)»** kartasi.
2. Kartadagi **«Qanday ulanadi?»** qo'llanmasini oching va bajaring (qisqacha):
   - [console.firebase.google.com](https://console.firebase.google.com) → **Add project** → nom bering.
   - **Build → Realtime Database → Create Database** → joy tanlang → **Start in locked mode**.
   - **Build → Authentication → Get started → Email/Password → Enable**.
   - **⚙ Project settings → Your apps → Web (`</>`)** → ilova qo'shing → `firebaseConfig` (`{ ... }` qismi) ni nusxalang.
3. Config'ni kartadagi katta maydonga joylang. Agar config'da `databaseURL` bo'lmasa — RTDB sahifasi tepasidagi `https://...firebasedatabase.app` ni alohida maydonga joylang. **Saqlash**.
4. **Email va parol** bilan **Ro'yxatdan o'tish**. Boshqa qurilmada xuddi shu config + shu email/parol bilan kiring — hammasi o'zi sinxronlanadi.
5. **Xavfsizlik** (RTDB → Rules), `database.rules.json` faylidagi qoidalarni qo'ying:
   ```json
   { "rules": { "users": { "$uid": { ".read": "$uid === auth.uid", ".write": "$uid === auth.uid" } } } }
   ```

> Header'dagi belgi holatni ko'rsatadi: **● Lokal** (ulanmagan) · **☁ ulanmagan** (config bor, kirilmagan) · **☁ sinxron** (yoniq). Belgini bossangiz — to'g'ri Sozlamalarga olib boradi.

---

## Saytni joylash (deploy)

### A — Firebase Hosting (tavsiya etiladi)
```bash
npm install -g firebase-tools
firebase login
# .firebaserc ichidagi BU_YERGA_PROJECT_ID ni o'z loyiha ID'ingizga almashtiring
firebase deploy
```
`firebase deploy` saytni ham, RTDB qoidalarini ham joylaydi. Manzil: `https://LOYIHA.web.app`.

### B — GitHub Pages
Repozitoriyga `index.html`, `sw.js`, `manifest.json`, `icon-*.png`, `apple-touch-icon.png`, `icon.svg` ni yuklang → **Settings → Pages → Deploy from a branch → main / root**. Backend (sinxron) baribir Firebase'da qoladi. (Email/Parol usuli uchun domen ruxsati shart emas.)

---

## Telefonda
- Saytni telefon brauzerida oching, Sozlamalarda kiring — ma'lumotlaringiz o'sha zahoti ko'rinadi.
- **Uyga qo'shish (Add to Home Screen)** — alohida belgi bilan to'liq ekranli "app" (PWA). Internet yo'q bo'lsa ham ishlaydi.

## Ma'lumotni ko'chirish
Lokaldagi yozuvlarni saqlab qolish uchun: **Sozlamalar → Zaxira (JSON)** bilan yuklab oling, yangi joyda kirib **Zaxiradan yuklash** orqali tiklang. (Kirganingizda lokal ma'lumot avtomatik bulutga ham yoziladi.)

## Bo'limlar
**Bugun** (kun halqasi, yozuv qo'shish/tahrirlash, sekundomer, shablonlar, daftar) · **Tahlil** (ko'rsatkichlar, yillik faollik xaritasi, grafiklar, naqshlar) · **Reja** (byudjet, qarz, maqsadlar) · **Sozlamalar** (Firebase, 1 tanga = so'm, eslatma, zaxira, statistika).

## Fayllar
| Fayl | Vazifasi |
|------|----------|
| `index.html` | Ilovaning o'zi (sinxron mantig'i ichida) |
| `manifest.json`, `sw.js`, `icon-*.png`, `apple-touch-icon.png`, `icon.svg` | PWA (uyga qo'shish + oflayn) |
| `firebase.json`, `.firebaserc`, `database.rules.json` | Firebase Hosting + Realtime Database deploy |
