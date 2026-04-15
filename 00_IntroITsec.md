Szia! Nagyon szívesen segítek a felkészülésben. A BME CrySyS Lab (Buttyán Levente) anyagai tényleg sűrűk, és szeretnek rákérdezni a pontos definíciókra, valamint az összefüggésekre. 

Mivel ez az első diasor (00-IntroITsec), ez az alapozás. Itt a legfontosabb, hogy tisztában légy a terminológiával és a biztonság menedzsment/etika elméleti alapjaival . 

Íme az első diasor részletes, kimásolható és tanulható magyarázata:

---

## 00. Diasor: Bevezetés az IT biztonságba

Ez a modul az IT biztonság legfontosabb alapfogalmait, a támadók és sebezhetőségek típusait, a kockázatalapú megközelítést, valamint a szakmai etikai kérdéseket fekteti le .

### 1. Az IT biztonság "drámája" és az Alapfogalmak
* **Koncepció:** Az IT biztonság egy folyamatos küzdelem a védők és a támadók között.
    * A **rendszertervező/üzemeltető** IT rendszereket hoz létre és üzemeltet .
    * Ezek a rendszerek elkerülhetetlenül **sebezhetőségeket (vulnerabilities)** tartalmaznak.
    * A **támadó (attacker)** célja, hogy ezeket a sebezhetőségeket kihasználva (exploit) **támadást (attack)** hajtson végre .
    * A sikeres támadás **kompromittált állapotot (compromised state)** eredményez, ami egy nem kívánt állapot, ahol a biztonsági követelmények sérülnek.
    * Ezt elkerülendő, az üzemeltetők **biztonsági mechanizmusokat (security controls)** vezetnek be, amik megszüntetik a sebezhetőségeket vagy gátolják a támadásokat .
* **Az IT biztonság általános célja:** A rendszert folyamatosan nem-kompromittált állapotban tartani a támadások **megelőzésével (prevention)**, **gyors észlelésével és reagálással (detection and reaction)**, valamint a **helyreállítással (incidenskezelés)** .

### 2. Biztonsági Követelmények (CIA és AAA triad)
Minden biztonsági incidens az alábbi követelmények valamelyikének sérülését jelenti:
* **CIA Triad (Adatbiztonság):**
    * **Confidentiality (Bizalmasság):** Csak a jogosultak férhetnek hozzá az információhoz. (Példa sérülésre: jelszivárgás ).
    * **Integrity (Integritás) + Authenticity (Hitelesség):** Az adatot nem lehet jogosulatlanul módosítani, és a forrása megbízhatóan igazolható. (Példa sérülésre: adatbázis átírása, ransomware titkosítás ).
    * **Availability (Rendelkezésre állás):** Az adatok/szolgáltatások a jogosultak számára mindig elérhetőek legyenek. (Példa sérülésre: DoS támadás ).
* **Rendszerszintű hozzáférés-vezérlés (Access Control):**
    * **Authentication (Hitelesítés):** Egy entitás (felhasználó) állítólagos identitásának ellenőrzése (pl. jelszó megadása).
    * **Authorization (Engedélyezés/Jogosultságkezelés):** Hozzáférési jogok meghatározása és kikényszerítése (ki mihez férhet hozzá).
    * **Accountability (Számonkérhetőség / Naplózás):** A cselekvések visszamenőleges bizonyíthatósága naplózás révén.

### 3. A Támadások felépítése és a Támadók
* **Kill Chain modell:** A kibertámadások nem egyetlen kattintásból állnak, hanem egy komplex folyamatból. Például: Információszerzés (Reconnaissance) -> Fegyveresítés (Weaponization) -> Kézbesítés (Delivery) -> Kihasználás (Exploitation) -> Telepítés (Installation) -> Parancs és Vezérlés (C2) -> Célok elérése .
* **Katalógusok:** A támadási mintákat (CAPEC) és a taktikákat/technikákat (MITRE ATT&CK) hatalmas adatbázisokban gyűjtik a védők számára.
* **Támadó profilok:** Képességeik (tudás, információ) és erőforrásaik (pénz) alapján osztályozzuk őket :
    * **Script kiddie:** Alacsony tudás, limitált erőforrás, mások által írt kódokat használ.
    * **Elégedetlen munkatárs / Belső támadó:** Kevés erőforrás, de **óriási információszerző képesség** (hiszen már bent van).
    * **Hacktivista csoport:** Ideológiai motiváció (nem feltétlen pénz), moderált erőforrások.
    * **Kiberbűnözői szervezet:** Gazdasági haszonszerzés a cél (pénz), komoly infrastruktúra és szakértelem .
    * **Állam által szponzorált (APT - Advanced Persistent Threat):** Kémkedés/szabotázs a cél. Végtelen pénz, egyedi K+F (saját malware-ek), hosszú rejtőzködés .
* **Célzott támadások és Zero-day:** Ahol az áldozat nem véletlen (pl. Stuxnet) . A **Zero-day** olyan sérülékenység, amit a gyártó még nem ismer, így nincs rá javítás. Drágák, ezért főleg állami szinten használják őket .
    * *Példa:* **Stuxnet (2010)** - Iráni urándúsító centrifugák fizikai tönkretétele (PLC átprogramozásával) .
    * *Példa:* **Duqu (2011)** - A BME CrySyS labor fedezte fel! .

### 4. Sebezhetőségek és Gyengeségek
A sebezhetőségek lehetnek technikai/logikai (hibás kód), fizikai (hozzáférhető router a folyosón), üzemeltetési (gyári jelszó bent hagyása) vagy személyi (megzsarolható admin) jellegűek .
* **Adatbázisok:** **CWE** (szoftver/hardver gyengeségek katalógusa) és **CVE** (konkrét, azonosított sebezhetőségek listája, pl. NIST NVD).
* **Miért vannak hibák?** Rendszerek óriási komplexitása, erőforrás/idő hiány a fejlesztésnél, hibás specifikációk .
* **Felelősségteljes nyilvánosságra hozatal (Responsible disclosure):** Ha egy etikus hacker talál egy hibát, először szól a gyártónak, ad egy ésszerű határidőt a javításra, és csak utána hozza nyilvánosságra, hogy ösztönözze a cégeket a javításra .

### 5. Kockázatalapú Megközelítés (Risk Management)
Mivel lehetetlen minden ellen védekezni, priorizálni kell a pénzt és az időt.
* **Fenyegetés (Threat):** Potenciális támadás egy érték (asset) ellen.
* **Kockázat (Risk) = Valószínűség (Likelihood) $\times$ Várható veszteség/Hatás (Impact)**.
* A folyamat lépései : 
    1. **Azonosítás** (mik az értékeink, ki a támadó, mik a sebezhetőségek?) .
    2. **Becslés** (kockázati mátrix felállítása).
    3. **Kezelés** (ellenintézkedések bevezetése, amíg a költségvetés bírja).
    4. **Monitorozás.**.

### 6. Miért nehéz az IT biztonság?
1.  **Szándékos támadás vs. Véletlen hiba:** A véletlen hiba (pl. leég a szerver) ellen passzív rendszerekkel lehet védekezni, de a támadó *kreatív* és szándékosan keresi a kiskapukat .
2.  **A "Leggyengébb láncszem" probléma:** A védőnek **minden** komponenst 100%-osan kell védenie (Trója falai). A támadónak elég egyetlen hibát (egy trójai falovat) találnia a bejutáshoz . A leggyengébb láncszem legtöbbször az ember.
3.  **Biztonság vs. Kényelem/Funkciók:** A biztonság növeli a költségeket és csökkenti a kényelmet/teljesítményt. A felhasználók sok funkciót akarnak $\rightarrow$ a funkció növeli a komplexitást $\rightarrow$ **a komplexitás a biztonság legnagyobb ellensége** .

### 7. Etikai kérdések
Az alkalmazott etika/mérnöki etika foglalkozik ezzel. Vannak egyértelmű esetek (szándékos támadás, rossz gyakorlatok alkalmazása) és **"Szürke zónás"** esetek :
* **Etikus hackelés:** Ha nincs szerződés, hiába "jószándékú", bűncselekmény lehet (Lásd: BKK hacker esete a F12-vel) .
* **Nyilvánosságra hozatal időzítése:** Ki mondja meg, mi a fair türelmi idő egy javításra? .
* **"Jószándékú" kártevők:** Pl. a *Hajime* féreg, ami loT eszközöket "fertőz meg", hogy befoltozza a biztonsági réseiket. Etikus-e ez, ha amúgy a fejlesztője távolról irányíthatja a hálózatot? .
* **Állami kiberműveletek leleplezése:** Mi van, ha kideríted, hogy egy "baráti" ország kémli meg a magyar cégeket? Publikálod? Szólsz nekik? .
* Ezekre az **ACM Code of Ethics** ad iránymutatást az informatikusok számára .

---

### 🛡️ Kritikus pontok (ZH-tippek)
* **Authentication vs. Authorization:** A hitelesítés azt vizsgálja, hogy *ki vagy te* (jelszó). Az engedélyezés azt, hogy *mit csinálhatsz a rendszerben* (jogosultságok). Ezt nagyon sokszor keverik, ZH-n tuti beugratós kérdés!
* **CIA triad:** Tudd konkrét támadástípusokhoz rendelni! Például: a DoS támadás melyiket sérti? Válasz: Rendelkezésre állás (Availability). Ransomware? Integritás (és esetleg availability, ha nem férsz hozzá az adathoz, de az adatmódosítás miatt elsősorban integritás).
* **Sebezhetőség (Vulnerability) vs. Fenyegetés (Threat) vs. Kockázat (Risk):** Ne keverd össze őket! A sebezhetőség a rendszer sajátja (pl. nyitott port). A fenyegetés egy *potenciális* támadás (pl. egy hacker kihasználhatja a portot). A kockázat pedig ezek matematikai/üzleti értéke ($Valószínűség \times Veszteség$).
* **BME büszkeség:** A **Duqu** malware felfedezését a CrySyS laborhoz (a tárgy előadóihoz) kötik, ezt szeretik visszakérdezni .

Szólj, ha tölthetjük a következő diasort!