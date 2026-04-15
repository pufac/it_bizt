Ez a második diasor a **Webbiztonságról** szól. Ez a terület azért kritikus, mert a webalkalmazások jelentik a vállalatok leginkább kitett felületét. A jegyzet az incidensekből indul ki, majd részletesen tárgyalja az **OWASP Top 10** (2025-ös verzió) legfontosabb elemeit, különös tekintettel az **Injection** (SQLi, XSS) és a **Broken Access Control** hibákra.

---

# IT Biztonság: Webbiztonság jegyzet

## 1. Webbiztonsági helyzetkép
A webes incidensek száma és hatása folyamatosan nő. 
* **Adatszivárgások:** Olyan óriásokat érintettek, mint a National Public Data (2,7 milliárd rekord), a Facebook (533 millió) vagy a Ticketmaster (560 millió).
* **Költségek:** A legdrágább iparág az egészségügy (átlagosan 9,77 millió USD szivárgásonként).
* **Kkv-k:** A támadást elszenvedő kisvállalkozások 60%-a 6 hónapon belül tönkremegy.

---

## 2. Az OWASP projekt és a Top 10 (2025)
Az **OWASP** (Open Worldwide Application Security Project) egy nonprofit szervezet, amely ingyenes, nyílt forrású anyagokkal segíti a szoftverbiztonságot .

### A 2025-ös lista főbb pontjai :
1.  **A01: Broken Access Control** (Hibás hozzáférés-védelem)
2.  **A02: Security Misconfiguration** (Biztonsági rosszkonfiguráció)
3.  **A03: Software Supply Chain Failures** (Szoftver ellátási lánc hibái) – **ÚJ!** 
4.  **A04: Cryptographic Failures** (Kriptográfiai hibák)
5.  **A05: Injection** (Beszúrásos támadások, pl. SQLi, XSS)

---

## 3. Hozzáférés-védelem és Konfiguráció (A01, A02)

### A01: Broken Access Control 
A felhasználó olyan műveletet végez, amihez nincs joga.
* **Példa:** A támadó kitalálja az admin felület URL-jét (`/app/admin_getappInfo`), amit csak az URL elrejtésével "védtek" .
* **Védekezés:** "Deny by default" elv (alapból tiltsunk mindent), könyvtárlistázás tiltása, szerver oldali ellenőrzés .

### A02: Security Misconfiguration 
Rosszul beállított rendszerek, alapértelmezett jelszavak, felesleges funkciók.
* **Hibaüzenetek:** Ha a rendszer részletes hibaüzenetet (stack trace) ad a felhasználónak, azzal értékes információt árul el a technológiai stackről.
* **Content Discovery:** A támadók olyan eszközökkel, mint a *Burp Suite* vagy a *ZAP Proxy*, megpróbálják feltérképezni a rejtett fájlokat .

---

## 4. Kliens oldali biztonság: SOP és CORS
A böngészők alapvető védelmi modellje az **Origin** fogalmára épül.
* **Origin = Séma + Domain + Port**.
* **Same Origin Policy (SOP):** Egy weboldal JS kódja csak azonos originről érhet el erőforrásokat . 
    * Beágyazás (pl. `<img>`, `<script>`) megengedett cross-origin is .
    * Olvasás (read) azonban csak azonos originről szabad.
* **Cross-Origin Resource Sharing (CORS):** Egy HTTP alapú mechanizmus, amivel a szerver engedélyezheti a böngészőnek, hogy a SOP-ot feloldja bizonyos más originek számára .
    * **Preflight kérés:** Bonyolultabb kérések (PUT, DELETE) előtt a böngésző egy `OPTIONS` kérést küld, hogy megnézze, engedélyezett-e a művelet .

---

## 5. Injection - SQL Injekció (A05)
Az **Injection** lényege: a felhasználói bemenet és a parancs (kód) összekeveredik az értelmezőben.


* **Hiba:** `query = f"SELECT * FROM users WHERE name='{username}'"`.
* **Támadás:** Ha a `username` értéke: `asd' OR '1'='1`, a feltétel mindig igaz lesz, és a támadó jelszó nélkül beléphet .
* **Query Stacking:** `;` használatával új parancs fűzhető hozzá, pl. `Robert'); DROP TABLE Students;--` (a híres "Little Bobby Tables" példa) .
* **Védekezés:** **Parameterized statements** (előkészített utasítások). A bemenet soha nem válik a kód részévé .

---

## 6. Injection - Cross-Site Scripting (XSS)
A támadó kártékony JavaScript kódot futtat az áldozat böngészőjében, az áldozat originjén belül.

### Típusai:
1.  **Stored (Persistent) XSS:** A kód a szerveren tárolódik (pl. adatbázisban egy kommentként). Mindenki fertőződik, aki megnézi az oldalt .
2.  **Reflected XSS:** A kód egy URL paraméterben van benne, és a szerver "visszatükrözi" az oldalra. Csak azt érinti, aki rákattint a linkre .
3.  **DOM-based XSS:** A hiba tisztán kliens oldalon van (pl. egy kliens oldali JS változóból írnak az `innerHTML`-be) .

### Védekezés:
* **Output Encoding:** A speciális karaktereket (`<`, `>`, `&`) át kell alakítani (pl. `&lt;`, `&gt;`), hogy a böngésző ne kódként kezelje őket .
* **HTTP-only süti:** Megakadályozza, hogy JS-ből el lehessen érni a munkamenet-azonosító (session) sütit, így az XSS-sel nem lopható el .
* **Content Security Policy (CSP):** Fejléc, amiben a szerver megmondja, honnan enged tölteni scripteket, és tiltja-e az inline JS-t .

---

## 7. Egyéb fontos pontok
* **Software Supply Chain (A03):** 2019-es SolarWinds támadás: malware került a frissítésbe, ami 18 000 szervezetet fertőzött meg. Megoldás: **SBOM** (Software Bill of Materials) használata.
* **Kriptográfiai hibák (A04):** Elavult hash-ek (pl. MD5, SHA1), sózás nélküli jelszótárolás, vagy kulcsok feltöltése Git-re .
* **Bug Bounty:** A gyártók (Google, Mozilla) pénzt fizetnek a felfedezett hibákért, hogy megelőzzék a feketepiaci értékesítést .

**Ellenőrző kérdés tipp vizsgára:** Mi a különbség a SOP és a CORS között? (SOP a tiltó mechanizmus, CORS a szabályozott engedmény).

Melyik legyen a következő diasor?

# sop és cors jobban mert nem tudtam mi ez

Semmi gond, a **SOP** és a **CORS** a webbiztonság talán legszárazabb, de legfontosabb része. Képzeld el úgy őket, mint egy szigorú kidobót (SOP) és egy vendéglistát (CORS) a böngésződben.

Íme a részletes magyarázat a diasor alapján:

### 1. Az Alapfogalom: Az Origin (Eredet)
Mielőtt megértenénk a szabályt, tudnunk kell, mi az az **Origin**. A böngésző három adatból azonosít egy weboldalt:
1.  **Séma** (pl. `http` vagy `https`) 
2.  **Domain** (pl. `example.com`) 
3.  **Port** (pl. `80` vagy `443`) 

Ha bármelyik eltér, az már **másik originnek** számít.

### 2. SOP (Same Origin Policy) – A Szigorú Kidobó
A **Same Origin Policy** egy alapvető biztonsági mechanizmus a böngészőben. Az alapelve egyszerű: egy weboldalról származó szkript (pl. JavaScript) csak a saját originjéből származó adatokhoz férhet hozzá korlátozás nélkül.

**Miért van erre szükség?**
Ha nem lenne SOP, akkor ha megnyitnál egy kártékony weboldalt, az a háttérben futó JavaScripttel simán kiolvashatná a mellette megnyitott Facebookodról a privát üzeneteidet vagy a banki adataidat, hiszen a böngésző "belépve" tart téged. A SOP ezt tiltja meg: a kártékony oldal nem "olvashat bele" egy másik oldal adataiba.

**Mit enged és mit tilt a SOP?** 
* **Beágyazás (Embedding) – ENGEDÉLYEZETT:** Bármelyik oldalról beölthetsz képet (`<img>`), videót vagy külső JavaScript fájlt (`<script>`).
* **Írás/Küldés (Writing) – ENGEDÉLYEZETT:** Egy linkre kattintva átmehetsz másik oldalra, vagy beküldhetsz egy űrlapot másik oldalnak.
* **Olvasás (Reading) – TILTOTT:** JavaScripttel nem tudod elolvasni egy másik originről jövő válasz (response) tartalmát.

---

### 3. CORS (Cross-Origin Resource Sharing) – A Vendéglista
Vannak esetek, amikor a fejlesztőnek **szüksége van** arra, hogy az egyik oldala (pl. `frontend.com`) adatokat kérjen le a másik oldaláról (pl. `api.backend.com`). Mivel ezek különböző originek, a SOP alapból blokkolná a választ. Itt jön képbe a **CORS**.

A CORS egy **szerver oldali beállítás**, amivel a szerver megmondja a böngészőnek: *"Figyi, én tudom, hogy a frontend.com nem én vagyok, de megbízom benne, nyugodtan add oda neki az adatokat"*.



**Hogyan működik a gyakorlatban?** 
1.  **Egyszerű kérések (pl. GET):** A böngésző elküldi a kérést, de a választ csak akkor engedi elolvasni a JS-nek, ha a szerver válaszában ott van az `Access-Control-Allow-Origin: http://frontend.com` fejléc.
2.  **Bonyolult kérések (pl. POST JSON-nel, PUT, DELETE):** Itt a böngésző óvatos. Előbb küld egy **Preflight** kérést (`OPTIONS` metódussal). Megkérdezi a szervert: *"Szabad nekem ilyet küldeni?"*. Ha a szerver igent mond, csak akkor megy el a valódi adat.

### Összefoglalva:
* **SOP:** A böngésző alapértelmezett védelme, ami mindent tilt, ami nem ugyanaz az origin.
* **CORS:** A kivételkezelés, amivel a szerver engedélyezheti a SOP feloldását bizonyos külső oldalak számára .

**Példa a vizsgához:**
Ha a `http://a.hu` akar adatot kérni a `https://a.hu`-tól (eltérő séma: http vs https), az **SOP hiba** lesz, hacsak a szerver nem küld **CORS** fejlécet.

Egyértelműbb így a különbség?folytassuk a következő diasorral?