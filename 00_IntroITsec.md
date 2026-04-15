Szia! Nagyon szívesen segítek a felkészülésben. [cite_start]A BME CrySyS Lab (Buttyán Levente) anyagai tényleg sűrűk, és szeretnek rákérdezni a pontos definíciókra, valamint az összefüggésekre[cite: 5, 6]. 

Mivel ez az első diasor (00-IntroITsec), ez az alapozás. [cite_start]Itt a legfontosabb, hogy tisztában légy a terminológiával és a biztonság menedzsment/etika elméleti alapjaival [cite: 12-15]. 

Íme az első diasor részletes, kimásolható és tanulható magyarázata:

---

## 00. Diasor: Bevezetés az IT biztonságba

[cite_start]Ez a modul az IT biztonság legfontosabb alapfogalmait, a támadók és sebezhetőségek típusait, a kockázatalapú megközelítést, valamint a szakmai etikai kérdéseket fekteti le [cite: 12-15].

### 1. Az IT biztonság "drámája" és az Alapfogalmak
* [cite_start]**Koncepció:** Az IT biztonság egy folyamatos küzdelem a védők és a támadók között[cite: 38].
    * [cite_start]A **rendszertervező/üzemeltető** IT rendszereket hoz létre és üzemeltet [cite: 39-42].
    * [cite_start]Ezek a rendszerek elkerülhetetlenül **sebezhetőségeket (vulnerabilities)** tartalmaznak[cite: 48, 54].
    * [cite_start]A **támadó (attacker)** célja, hogy ezeket a sebezhetőségeket kihasználva (exploit) **támadást (attack)** hajtson végre [cite: 51-58].
    * [cite_start]A sikeres támadás **kompromittált állapotot (compromised state)** eredményez, ami egy nem kívánt állapot, ahol a biztonsági követelmények sérülnek[cite: 59, 63, 64].
    * [cite_start]Ezt elkerülendő, az üzemeltetők **biztonsági mechanizmusokat (security controls)** vezetnek be, amik megszüntetik a sebezhetőségeket vagy gátolják a támadásokat [cite: 43-46].
* [cite_start]**Az IT biztonság általános célja:** A rendszert folyamatosan nem-kompromittált állapotban tartani a támadások **megelőzésével (prevention)**, **gyors észlelésével és reagálással (detection and reaction)**, valamint a **helyreállítással (incidenskezelés)** [cite: 115-120].

### 2. Biztonsági Követelmények (CIA és AAA triad)
[cite_start]Minden biztonsági incidens az alábbi követelmények valamelyikének sérülését jelenti[cite: 99]:
* **CIA Triad (Adatbiztonság):**
    * [cite_start]**Confidentiality (Bizalmasság):** Csak a jogosultak férhetnek hozzá az információhoz[cite: 87, 88]. [cite_start](Példa sérülésre: jelszivárgás [cite: 101]).
    * [cite_start]**Integrity (Integritás) + Authenticity (Hitelesség):** Az adatot nem lehet jogosulatlanul módosítani, és a forrása megbízhatóan igazolható[cite: 89, 90]. [cite_start](Példa sérülésre: adatbázis átírása, ransomware titkosítás [cite: 103, 109]).
    * [cite_start]**Availability (Rendelkezésre állás):** Az adatok/szolgáltatások a jogosultak számára mindig elérhetőek legyenek[cite: 91]. [cite_start](Példa sérülésre: DoS támadás [cite: 111]).
* **Rendszerszintű hozzáférés-vezérlés (Access Control):**
    * [cite_start]**Authentication (Hitelesítés):** Egy entitás (felhasználó) állítólagos identitásának ellenőrzése (pl. jelszó megadása)[cite: 71, 72].
    * [cite_start]**Authorization (Engedélyezés/Jogosultságkezelés):** Hozzáférési jogok meghatározása és kikényszerítése (ki mihez férhet hozzá)[cite: 69, 70, 75].
    * [cite_start]**Accountability (Számonkérhetőség / Naplózás):** A cselekvések visszamenőleges bizonyíthatósága naplózás révén[cite: 73, 78].

### 3. A Támadások felépítése és a Támadók
* [cite_start]**Kill Chain modell:** A kibertámadások nem egyetlen kattintásból állnak, hanem egy komplex folyamatból[cite: 124, 125]. [cite_start]Például: Információszerzés (Reconnaissance) -> Fegyveresítés (Weaponization) -> Kézbesítés (Delivery) -> Kihasználás (Exploitation) -> Telepítés (Installation) -> Parancs és Vezérlés (C2) -> Célok elérése [cite: 127-146].
* [cite_start]**Katalógusok:** A támadási mintákat (CAPEC) és a taktikákat/technikákat (MITRE ATT&CK) hatalmas adatbázisokban gyűjtik a védők számára[cite: 148, 150, 165, 183].
* [cite_start]**Támadó profilok:** Képességeik (tudás, információ) és erőforrásaik (pénz) alapján osztályozzuk őket [cite: 188, 194, 240-258]:
    * [cite_start]**Script kiddie:** Alacsony tudás, limitált erőforrás, mások által írt kódokat használ[cite: 252, 255].
    * [cite_start]**Elégedetlen munkatárs / Belső támadó:** Kevés erőforrás, de **óriási információszerző képesség** (hiszen már bent van)[cite: 252].
    * [cite_start]**Hacktivista csoport:** Ideológiai motiváció (nem feltétlen pénz), moderált erőforrások[cite: 251, 254].
    * [cite_start]**Kiberbűnözői szervezet:** Gazdasági haszonszerzés a cél (pénz), komoly infrastruktúra és szakértelem [cite: 259-270].
    * **Állam által szponzorált (APT - Advanced Persistent Threat):** Kémkedés/szabotázs a cél. [cite_start]Végtelen pénz, egyedi K+F (saját malware-ek), hosszú rejtőzködés [cite: 272-284].
* [cite_start]**Célzott támadások és Zero-day:** Ahol az áldozat nem véletlen (pl. Stuxnet) [cite: 386-388]. A **Zero-day** olyan sérülékenység, amit a gyártó még nem ismer, így nincs rá javítás. [cite_start]Drágák, ezért főleg állami szinten használják őket [cite: 379-384].
    * [cite_start]*Példa:* **Stuxnet (2010)** - Iráni urándúsító centrifugák fizikai tönkretétele (PLC átprogramozásával) [cite: 392-405].
    * [cite_start]*Példa:* **Duqu (2011)** - A BME CrySyS labor fedezte fel! [cite: 407-414].

### 4. Sebezhetőségek és Gyengeségek
[cite_start]A sebezhetőségek lehetnek technikai/logikai (hibás kód), fizikai (hozzáférhető router a folyosón), üzemeltetési (gyári jelszó bent hagyása) vagy személyi (megzsarolható admin) jellegűek [cite: 287-291].
* [cite_start]**Adatbázisok:** **CWE** (szoftver/hardver gyengeségek katalógusa) és **CVE** (konkrét, azonosított sebezhetőségek listája, pl. NIST NVD)[cite: 294, 296, 317, 334].
* [cite_start]**Miért vannak hibák?** Rendszerek óriási komplexitása, erőforrás/idő hiány a fejlesztésnél, hibás specifikációk [cite: 363-367].
* [cite_start]**Felelősségteljes nyilvánosságra hozatal (Responsible disclosure):** Ha egy etikus hacker talál egy hibát, először szól a gyártónak, ad egy ésszerű határidőt a javításra, és csak utána hozza nyilvánosságra, hogy ösztönözze a cégeket a javításra [cite: 374-377].

### 5. Kockázatalapú Megközelítés (Risk Management)
[cite_start]Mivel lehetetlen minden ellen védekezni, priorizálni kell a pénzt és az időt[cite: 464, 465].
* [cite_start]**Fenyegetés (Threat):** Potenciális támadás egy érték (asset) ellen[cite: 466].
* [cite_start]**Kockázat (Risk) = Valószínűség (Likelihood) $\times$ Várható veszteség/Hatás (Impact)**[cite: 467].
* [cite_start]A folyamat lépései [cite: 452-461]: 
    1. [cite_start]**Azonosítás** (mik az értékeink, ki a támadó, mik a sebezhetőségek?) [cite: 475-478].
    2. [cite_start]**Becslés** (kockázati mátrix felállítása)[cite: 478, 489].
    3. [cite_start]**Kezelés** (ellenintézkedések bevezetése, amíg a költségvetés bírja)[cite: 468, 469].
    4. [cite_start]**Monitorozás.**[cite: 459].

### 6. Miért nehéz az IT biztonság?
1.  [cite_start]**Szándékos támadás vs. Véletlen hiba:** A véletlen hiba (pl. leég a szerver) ellen passzív rendszerekkel lehet védekezni, de a támadó *kreatív* és szándékosan keresi a kiskapukat [cite: 504-510].
2.  **A "Leggyengébb láncszem" probléma:** A védőnek **minden** komponenst 100%-osan kell védenie (Trója falai). [cite_start]A támadónak elég egyetlen hibát (egy trójai falovat) találnia a bejutáshoz [cite: 588-591, 596-598]. [cite_start]A leggyengébb láncszem legtöbbször az ember[cite: 592].
3.  [cite_start]**Biztonság vs. Kényelem/Funkciók:** A biztonság növeli a költségeket és csökkenti a kényelmet/teljesítményt[cite: 602, 603]. [cite_start]A felhasználók sok funkciót akarnak $\rightarrow$ a funkció növeli a komplexitást $\rightarrow$ **a komplexitás a biztonság legnagyobb ellensége** [cite: 605-607].

### 7. Etikai kérdések
[cite_start]Az alkalmazott etika/mérnöki etika foglalkozik ezzel[cite: 611, 616, 619]. [cite_start]Vannak egyértelmű esetek (szándékos támadás, rossz gyakorlatok alkalmazása) és **"Szürke zónás"** esetek [cite: 622-627]:
* [cite_start]**Etikus hackelés:** Ha nincs szerződés, hiába "jószándékú", bűncselekmény lehet (Lásd: BKK hacker esete a F12-vel) [cite: 644-648].
* [cite_start]**Nyilvánosságra hozatal időzítése:** Ki mondja meg, mi a fair türelmi idő egy javításra? [cite: 658-662].
* **"Jószándékú" kártevők:** Pl. a *Hajime* féreg, ami loT eszközöket "fertőz meg", hogy befoltozza a biztonsági réseiket. [cite_start]Etikus-e ez, ha amúgy a fejlesztője távolról irányíthatja a hálózatot? [cite: 669-679].
* **Állami kiberműveletek leleplezése:** Mi van, ha kideríted, hogy egy "baráti" ország kémli meg a magyar cégeket? Publikálod? [cite_start]Szólsz nekik? [cite: 681-691].
* [cite_start]Ezekre az **ACM Code of Ethics** ad iránymutatást az informatikusok számára [cite: 693-709].

---

### 🛡️ Kritikus pontok (ZH-tippek)
* **Authentication vs. Authorization:** A hitelesítés azt vizsgálja, hogy *ki vagy te* (jelszó). [cite_start]Az engedélyezés azt, hogy *mit csinálhatsz a rendszerben* (jogosultságok)[cite: 69, 72]. Ezt nagyon sokszor keverik, ZH-n tuti beugratós kérdés!
* **CIA triad:** Tudd konkrét támadástípusokhoz rendelni! Például: a DoS támadás melyiket sérti? [cite_start]Válasz: Rendelkezésre állás (Availability)[cite: 91, 111]. Ransomware? [cite_start]Integritás (és esetleg availability, ha nem férsz hozzá az adathoz, de az adatmódosítás miatt elsősorban integritás)[cite: 109, 112].
* **Sebezhetőség (Vulnerability) vs. Fenyegetés (Threat) vs. Kockázat (Risk):** Ne keverd össze őket! [cite_start]A sebezhetőség a rendszer sajátja (pl. nyitott port)[cite: 54, 287]. [cite_start]A fenyegetés egy *potenciális* támadás (pl. egy hacker kihasználhatja a portot)[cite: 466]. [cite_start]A kockázat pedig ezek matematikai/üzleti értéke ($Valószínűség \times Veszteség$)[cite: 467].
* [cite_start]**BME büszkeség:** A **Duqu** malware felfedezését a CrySyS laborhoz (a tárgy előadóihoz) kötik, ezt szeretik visszakérdezni [cite: 407-414].

Szólj, ha tölthetjük a következő diasort!