# Multi-Restaurant QR Ordering v2 (Free: GitHub Pages + Firebase Spark)

Ek hi app, har restaurant/cafe ka alag naam, menu, offer, colour aur owner login.

## v2 mein kya naya / theek hua
1. **"Order more" bug fix**: ab pichla order band nahi hota. Customer ke saare orders ek jagah, ek total ke saath dikhte hain.
2. **Jhootha "Order placed" band**: internet nahi hai to order bhejne se pehle hi bata deta hai. Order tabhi "placed" dikhta hai jab server pe save ho jaye.
3. **Security**: PIN hata diya. Owner email + password se login (email verify zaroori). Firebase rules locked: customer sirf apne orders dekh sakta hai, phone numbers sirf owner ko dikhte hain, 15 second mein ek hi order/request (spam control).
4. **Naye order pe awaaz + vibration + toast + tab title blink**. Waiter/Bill request pe bhi. Screen on rehti hai (supported phones pe).
5. **Menu**: categories, search, veg/non-veg 🟢🔴 mark, "Abhi khatam" (out of stock). Order se pehle **cart review**, quantity badlo, **special instruction** (kam teekha, Jain…), total GST ke saath pehle hi dikhta hai.
6. **🙋 Waiter bulao / 🧾 Bill mangao** buttons. App se koi payment nahi hota — payment cafe staff ko (cafe apna scanner WhatsApp pe bhejta hai).
7. Extra: order cancel, aaj ki sale, tables count setting, WhatsApp offers sirf un customers ko jinhone haan bola, purane bills ka GST % nahi badalta, "WOW" naam hata diya.

## Setup (ek baar)
1. **Firebase** (console.firebase.google.com): project banao.
2. **Authentication → Sign-in method**: **Anonymous** aur **Email/Password** dono ON karo.
3. **Realtime Database** banao → **Rules** tab mein `database.rules.json` ka pura content paste karke Publish.
4. Project settings → Web app register karo → `firebaseConfig` (databaseURL ke saath) copy karke `index.html` ke `FIREBASE_CONFIG` mein paste karo.
5. Authentication → Settings → **Authorized domains** mein `<user>.github.io` add karo.
6. GitHub pe public repo → `index.html` upload → Settings → Pages → `main` / root.

## Apna reseller (admin) account
1. `https://<user>.github.io/<repo>/#super` kholo → "Naya account" se apna account banao.
2. Screen pe tumhara UID dikhega. Firebase console → Realtime Database → root pe `admins` → key = UID, value = `true` add karo. Reload.

## Naya restaurant add karna (2 minute)
1. `#super` → naam, short-id (jaise `sahara-cafe`), **owner ka email** → Create.
2. Owner ko do: `.../?r=sahara-cafe#admin` (dashboard) aur `.../?r=sahara-cafe#qr` (table QR).
3. Owner dashboard pe "Naya account" se **wahi email** use karke account banata hai, email verify karta hai, login.
4. Owner khud: **🍽 Menu** (dishes, category, veg, available), **📣 Offers**, **⚙ Setup** (naam, colour, GST, WhatsApp, tables).
5. Kitchen tablet pe dashboard khol ke ek baar **🔕 Sound ON karo** dabana (browser ka rule hai).

## Links
- Customer: QR scan → `?r=sahara-cafe#table5`
- Owner: `?r=sahara-cafe#admin`
- Tum (reseller): `#super`

## Purane (v1) restaurants
Har restaurant ke liye database mein `restaurants/<id>/owner` = owner ka email (lowercase) add karo. Purana menu apne aap naye format mein chal jayega (category "Menu"). Config mein pada purana `pin` owner ke pehle Save pe hat jayega.

## Abhi bhi dhyan do
- Koi bhi link khol ke ghar se order bhej sakta hai (15 sec limit hai). Zyada strict chahiye to Firebase **App Check** lagao.
- Dashboard sirf pichhle 36 ghante ke orders sync karta hai (fast + free plan limits). Lambi reports agla step hai.
- Invoice abhi GST-compliant nahi hai (GSTIN, CGST/SGST split, sequential number). Paid clients se pehle ye karna.

## v2.1: WhatsApp bill + Google review
- Setup mein **Restaurant ka WhatsApp number** aur **Google review link** daalo.
- Customer ke order screen pe "📲 Bill WhatsApp pe lo" button aata hai. Customer apne WhatsApp se restaurant ko message bhejta hai (Table no., total, order ID ke saath), to number 100% sahi milta hai. Owner usi chat mein bill bhej de (Invoice → Print/screenshot ya bill text paste).
- Customer 4–5 star de to "⭐ Google pe review do" button; 1–3 star pe sorry message (Google pe nahi bhejta).
- Google review link: Google Business Profile → "Ask for reviews" / "Get more reviews" → link copy.

## v2.2: Owner + customer review fixes
**Owner**
- Live orders ab **table-wise** group hote hain: har table ka total, ek **🧾 Table bill** (saare rounds ek invoice mein) aur **💰 Paid & close table**.
- Order pe **✅ Accept** → **🍽 Ready / Served** → table close.
- **🖨 KOT** button: kitchen ke liye bade font mein slip.
- **⚠ Rate check**: agar order ka rate menu se alag ho (koi link se chhed-chhaad kare) ya dish menu mein na ho, laal warning.
- Table close karte hi us table ke bill/payment alerts apne aap hat jaate hain.

**Customer**
- Order cancel hua to customer ko saaf message dikhta hai (pehle chupchaap gayab ho jata tha).
- Bill close hone ke baad bhi rating + Google review ka mauka.
- Purana "Shukriya" message agli visit pe nahi dikhta (sirf 4 ghante tak).
- Live site pe "Demo" wala text nahi dikhta.

**Upgrade**: `index.html` (config wapas daalna) aur `database.rules.json` dono dobara publish karo.

## v2.3: Menu copy / bulk import
- `#super` mein naya cafe banate waqt **Menu** chuno: Sample, Khaali, ya kisi purane cafe se **Copy**.
- Reseller apne login se kisi bhi cafe ka `?r=<id>#admin` khol ke menu bhar sakta hai.
- Menu tab → **📋 Bulk menu**: har line `Dish, Rate, Category, veg/nonveg` paste karo → "Upar add karo" ya "Poora menu replace" → **💾 Save menu**. Chicken/mutton/fish/egg naam se non-veg apne aap. "📤 Current menu text" se menu copy karke doosre cafe mein paste kar sakte ho.

## v3: Ek hi app, do login
- Main link (`/menu/`) pe do login: **Hotel / Cafe login** aur **Reseller login**. Login ke baad app khud pehchanta hai: reseller ko reseller panel, owner ko seedha apna dashboard.
- Reseller sirf ek: `admins/<uid>` (sirf Firebase console/CLI se set hota hai).
- Naya cafe banate waqt reseller hi owner ka email + password banata hai, phir "Owner ko ye bhejo" se WhatsApp pe login details. Owner ko signup/verify nahi karna.
- Reseller panel: 🔑 Login (naya login / reset link), ✉ Owner badlo, 🗑 Delete cafe.
- Owner dashboard mein 🔳 QR tab: har table ka ⬇ Download (PNG), ⬇ Sab download, 🖨 Sab print.
- Menu: upar quick add (naam, rate, category, veg), Enter se add, ✅ Hai / ❌ Khatam ek tap, ✏ edit, 🗑 delete, category rename, sab auto-save. Bulk paste bhi.
- Rules deploy: `npx firebase-tools deploy --only database --project qr-menu-f5e9a` (firebase.json saath hai).

## v3.1: App + custom options
- PWA app: manifest.json, sw.js, icons. Owner main link Chrome mein khole → "📲 App install karo" (ya ⋮ → Install app). iPhone: Safari Share → Add to Home Screen. Play Store nahi chahiye.
- Custom options (owner khud banata hai): Menu → dish ✏ → "⚙ Custom options". Har line ek group: `Size: Half 0, Full 80` (ek chuno), `Add-ons+: Extra cheese 30` (+ = kai chun sakte). Rate = base ke upar extra. Quick buttons: Half/Full, S/M/L, Add-ons, Spice.
- Customer: options wali dish pe "+ Add" se chooser khulta hai, live price; order/KOT/bill mein "Paneer Tikka (Full, Extra cheese)" dikhta hai.
