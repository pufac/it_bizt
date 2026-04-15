Ez a diasor a szoftverbiztonság alapjaival és a **Security Development Lifecycle (SDL)** folyamatával foglalkozik. A cél a biztonság beépítése a szoftverfejlesztés minden fázisába, nem pedig utólagos „foltozásként” kezelni azt.

---

# Szoftverbiztonság és SDL

## 1. A szoftverbiztonság helyzete manapság
A modern szoftverfejlesztésben hatalmas kihívást jelent a sérülékenységek kezelése.
* **CVE (Common Vulnerabilities and Exposures):** Egy nyilvános adatbázis a megtalált sérülékenységekről. A rekordok száma drasztikusan nő: átlagosan **12 percenként** kerül be egy új sérülékenység a rendszerbe.
* **Kihívások:**
    * **Lejt a pálya:** A fejlesztőket korlátozza az idő, az erőforrások és a funkcionalitás kényszere, míg a támadóknak csak motivációra és felkészültségre van szükségük.
    * **Biztonsági tesztelés nehézsége:** A funkcionális tesztelés azt nézi, hogyan *kell* működnie a rendszernek, a biztonsági tesztelés viszont azt, hogyan *nem szabadna* működnie.
    * **Gazdasági tényezők:** Gyakran gyenge az üzleti motiváció, és a károkat sokszor a végfelhasználó szenvedi el, nem a fejlesztő.
* **A jó hír:** Az incidensek 90%-a ugyanazokra a jól ismert okokra vezethető vissza, amik ellen léteznek hatásos és olcsó védekezési mechanizmusok.

---

## 2. Microsoft SDL (Security Development Lifecycle)
A Microsoft által kidolgozott módszertan célja a sérülékenységek számának és súlyosságának csökkentése a fejlesztés során.

### A folyamat fázisai:
1.  **SDL előtt (Tréning):** A fejlesztők biztonsági oktatása alapvető.
2.  **Követelmények:** A biztonsági és adatvédelmi (privacy) követelmények meghatározása ($CIA+3A$).
3.  **Tervezés:** Fenyegetésmodellezés és biztonsági alapelvek alkalmazása.
4.  **Implementáció:** Biztonságos kódolás, statikus elemzés.
5.  **Verifikáció:** Dinamikus elemzés, tesztelés, fuzzing.
6.  **Kiadás:** Security response terv készítése.
7.  **SDL után (Válasz):** Üzemeltetés közbeni incidenskezelés és javítások.

---

## 3. Tervezési alapelvek (Saltzer & Schroeder)
Ezek az elvek segítenek abban, hogy a szoftver architektúrája alapból ellenálló legyen.

* **Economy of Mechanism (KISS):** A tervek legyenek a lehető legegyszerűbbek. Minél komplexebb egy szoftver, annál valószínűbbek a hibák.
* **Fail-safe Defaults:** Alapértelmezésben tiltsunk meg mindent (engedélyezőlista), és csak a szükséges hozzáféréseket adjuk meg. Így egy hiba esetén a rendszer zárt marad.
* **Complete Mediation:** Minden egyes hozzáférést minden objektumhoz ellenőrizni kell, nem elég csak az első alkalommal (kerüljük a cache-elt jogosultságokat).
* **Open Design:** A biztonság ne a tervek titkosságán alapuljon (*security by obscurity* elkerülése). A nyilvános, átvizsgált tervek megbízhatóbbak.
* **Least Privilege:** Minden folyamat és felhasználó csak a feladata elvégzéséhez feltétlenül szükséges minimális jogosultságokkal fusson.
* **Separation of Privilege:** Egy kritikus művelethez több feltétel vagy kulcs együttes megléte kelljen (pl. kétfaktoros hitelesítés).
* **Least Common Mechanism:** Minimalizáljuk a felhasználók által közösen használt mechanizmusokat, mert ezek információszivárgási csatornák lehetnek.
* **Psychological Acceptability:** A biztonsági mechanizmusok ne nehezítsék meg túlságosan a felhasználó dolgát, különben megpróbálja majd megkerülni őket.

---

## 4. Implementációs fázis: Adatfeldolgozás
A legtöbb hiba a bemeneti adatok rossz kezeléséből adódik.

### A helyes adatkezelési folyamat:
1.  **Normalizáció / Kanonikalizáció:** Az adatot hozzuk a legegyszerűbb, egységes formára (pl. dátum formátum: YYYY-MM-DD), hogy elkerüljük a kijátszást.
2.  **Szűrés:** A tiltott elemek eltávolítása.
3.  **Validáció:** Ellenőrizzük az adatok ésszerűségét (típus, hossz, értéktartomány).
4.  **Semlegesítés (Escape-elés):** A kimenet formázása úgy, hogy az a következő komponens (pl. SQL szerver vagy böngésző) számára ne legyen káros.

> **Példa az aritmetikai hibára:** Ha egy programban két nagy számot adunk össze, előfordulhat **integer overflow**. Például ha a maximális pozitív egész számhoz hozzáadunk egyet, az eredmény egy negatív szám lehet, ami súlyos logikai hibákhoz vagy biztonsági résekhez vezet.

---

## 5. Naplózás és Hibakezelés
* **Hibakezelés:** Kerüljük a túl tág hibaüzeneteket, mert azok belső információkat árulhatnak el a támadónak (*information leakage*).
* **Naplózás (Logging):** Fontos az incidensek kivizsgálásához.
    * **Mit naplózzunk?** Ki, mikor, hol és mit csinált, különösen a hibákat és az autentikációs eseményeket.
    * **Mit NE naplózzunk?** Jelszavakat, privát kulcsokat, session tokeneket vagy forráskódot.
    * **Veszély:** A naplófájlokba is be lehet injektálni hamis bejegyzéseket, ha a bemenetet (pl. felhasználónév) nem validáljuk naplózás előtt.

---

## 6. Tesztelési módszerek
Dijkstra híres mondása szerint a tesztelés csak a hibák jelenlétét tudja bizonyítani, a hiányukat nem.

* **Statikus elemzés:** A kód futtatás nélküli átvizsgálása (automatizált eszközökkel). Jól skálázódik, de sok a fals pozitív eredmény.
* **Dinamikus elemzés:** A szoftver futás közbeni vizsgálata. Pontosabb, de alacsonyabb lehet a kódlefedettsége.
* **Fuzzing:** Automatikusan generált, véletlenszerű vagy hibás bemenetekkel bombázzuk a szoftvert, és figyeljük, összeomlik-e.
* **Penetrációs tesztelés:** Egy etikus hacker megpróbálja ténylegesen feltörni a rendszert, hogy demonstrálja a gyengeségeket.

---

## 7. SDL utáni fázisok
A szoftver kiadása után is szükség van biztonsági feladatokra.
* **Security Response:** Egy dedikált központ (SRC) kezeli a beérkező sérülékenység-jelentéseket, és koordinálja a javítások (patchek) elkészítését.
* **Incidenskezelés:** Ha bekövetkezik a baj (támadás vagy felelőtlen nyilvánosságra hozatal), egy gyorsreagálású csapatnak kell kezelnie a szituációt a károk minimalizálása érdekében.