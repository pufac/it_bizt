A negyedik diasor az **AAA** koncepciójáról szól, ami az informatikai biztonság egyik legfontosabb alapköve. Ez a betűszó a **Hitelesítés (Authentication)**, **Jogosultságkezelés (Authorization)** és **Elszámoltathatóság (Accounting)** hármasát takarja.

Íme a részletes jegyzet a felkészüléshez:

---

# IT Biztonság: AAA (Authentication, Authorization, Accounting)

Az AAA modell célja, hogy kontrollált keretek között tartsa az erőforrásokhoz való hozzáférést és nyomon követhetővé tegye a felhasználók tevékenységét.

## 1. Az AAA modell elemei

### **Authentication (Hitelesítés)**
Annak a folyamata, amely során a rendszer meggyőződik a felhasználó (vagy eszköz) személyazonosságáról. Ez a "Ki vagy te?" kérdésre ad választ.
* **Típusai:**
    * **Amit tudsz:** Jelszó, PIN kód, biztonsági kérdés.
    * **Ami nálad van:** Token, okoskártya, mobiltelefon (SMS kód), hardveres kulcs.
    * **Ami te vagy (biometria):** Ujjlenyomat, arcfelismerés, írisz-szkenner.
* **Többtényezős hitelesítés (MFA):** Legalább kettő különböző típusú azonosító együttes használata a biztonság növelése érdekében.

### **Authorization (Jogosultságkezelés)**
Miután a hitelesítés megtörtént, ez a folyamat határozza meg, hogy a felhasználónak mihez van joga a rendszerben. Ez a "Mit tehetsz?" kérdésre válaszol.
* **Példa:** Egy egyetemi rendszerben a hallgató csak a saját jegyeit láthatja, míg az oktató rögzítheti is azokat.
* **Alapelvek:**
    * **Legkisebb jogosultság elve (Least Privilege):** Mindenki csak a feladatai elvégzéséhez feltétlenül szükséges jogokat kapja meg.

### **Accounting (Elszámoltathatóság / Naplózás)**
A felhasználói tevékenységek rögzítése és nyomon követése. Ez a "Mit csináltál és mikor?" kérdésre felel.
* **Célja:** Biztonsági incidensek utólagos kivizsgálása, számlázás, statisztikák készítése.
* **Tartalma:** Bejelentkezés ideje, megnyitott fájlok, módosított adatok, hálózati forgalom mértéke.

---

## 2. Központosított AAA protokollok

A hálózati eszközök (routerek, switchek, VPN-ek) gyakran nem maguk tárolják a felhasználói adatokat, hanem egy központi AAA szerverhez fordulnak.



### **RADIUS (Remote Authentication Dial-In User Service)**
* **Jellemzői:**
    * UDP alapú protokoll.
    * Csak a jelszót titkosítja az átvitel során, a csomag többi részét nem.
    * Széles körben elterjedt (pl. Wi-Fi hitelesítésnél, VPN-eknél).
    * Az Authentication és Authorization folyamatokat egy lépésben kezeli.

### **TACACS+ (Terminal Access Controller Access-Control System Plus)**
* **Jellemzői:**
    * TCP alapú protokoll.
    * A teljes csomagot titkosítja, így biztonságosabb.
    * Különválasztja az AAA három elemét (külön kezelhető a hitelesítés és a jogosultság).
    * Főként hálózati eszközök (Cisco) adminisztrációjára használják.

---

## 3. Egyszeri bejelentkezés (Single Sign-On - SSO)

Az SSO lehetővé teszi, hogy a felhasználó egyszer jelentkezzen be, és utána több, egymástól független rendszert is használhasson anélkül, hogy újra jelszót kellene gépelnie.

* **Előnyei:**
    * Jobb felhasználói élmény.
    * Kevesebb elfelejtett jelszó, kevesebb segítségkérés a Helpdesktől.
    * Központi szabályozás és gyors jogosultságmegvonás (ha valaki távozik, elég egy helyen letiltani).
* **Kockázata:** "Single point of failure" – ha az SSO fiók kompromittálódik, a támadó az összes kapcsolódó rendszerhez hozzáfér.

### **Gyakori technológiák:**
* **Kerberos:** Belső hálózatokban (pl. Windows Active Directory) használt jegy-alapú rendszer.
* **SAML / OAuth 2.0 / OpenID Connect:** Webes szolgáltatások közötti hitelesítésre (pl. "Bejelentkezés Google-fiókkal").

---

**Vizsga-tipp:** Gyakori kérdés a **RADIUS és a TACACS+ összehasonlítása**. Jegyezd meg: RADIUS = UDP + csak jelszó titkosítás; TACACS+ = TCP + teljes titkosítás.

Mehetünk tovább az ötödik diasorra?