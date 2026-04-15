A harmadik diasor a **Biztonsági tesztelésről** szól. Ez a téma azért fontos, mert ahogy Edsger W. Dijkstra híres mondása tartja: *"A tesztelés hatékonyan megmutathatja a hibák jelenlétét, de reménytelenül alkalmatlan a hiányuk bizonyítására"* . A cél tehát a kockázatok minimalizálása és a sebezhetőségek minél korábbi feltárása.

Íme a részletes jegyzet a felkészüléshez:

---

# IT Biztonság: Biztonsági tesztelés jegyzet

## 1. Alapfogalmak és Megközelítések
A biztonsági tesztelés annak ellenőrzése, hogy a biztonsági követelmények teljesülnek-e.

### Tesztelési dimenziók :
* **Statikus vs. Dinamikus:**
    * **Statikus:** A forráskódot vagy a terveket elemzi futtatás nélkül.
    * **Dinamikus:** A szoftvert futás közben, valós körülmények között vizsgálja.
* **Black-box / White-box / Gray-box:**
    * **Black-box:** Csak a specifikációt ismeri, a belső működést nem (támadói nézőpont).
    * **White-box:** Ismeri az implementációt és a forráskódot.
    * **Gray-box:** A kettő kombinációja.
* **Pozitív vs. Negatív tesztelés:**
    * **Pozitív:** Azt nézzük, hogy az elvárt funkciók működnek-e.
    * **Negatív:** Azt vizsgáljuk, amit a rendszernek *nem* szabadna megengednie (pl. jogosulatlan hozzáférés).


---

## 2. Statikus Programanalízis (SAST)
A **SAST (Static Application Security Testing)** az "automatizált code review".

* **Működése:** A forráskódot vagy binárist egy belső reprezentációvá (pl. absztrakt szintaxis fa) alakítja, majd minták és heurisztikák alapján keres hibákat .
* **Mit keres?**
    * Veszélyes függvények (pl. `strcpy`).
    * Injection lehetőségek, integer túlcsordulások .
    * Nyelvspecifikus hibák (pl. Java üres catch blokkok).
* **Előnyök/Hátrányok:** Skálázható és már a fejlesztés elején használható, de sok a **hamis pozitív** (téves riasztás) eredménye .

---

## 3. Dinamikus Programanalízis (DAST) és Fuzzing
A **DAST (Dynamic Application Security Testing)** futás közben figyeli a szoftvert.

### Fuzzing (Fuzz tesztelés) :
* **Alapelv:** Automatikusan generált, véletlenszerű vagy félig strukturált bemenetekkel bombázzuk a rendszert, és figyeljük, mikor omlik össze (crash) vagy akad le (timeout).
* **Cél:** Elérni a "mély" kódrészleteket, ahol ritka hibák bújhatnak meg.
* **Eszközök:** Pl. *American Fuzzy Lop (AFL)*.

### SAST vs DAST összehasonlítás :
| Szempont | SAST (Statikus) | DAST (Dinamikus) |
| :--- | :--- | :--- |
| **Lefedettség** | Nagy (minden kódsort láthat) | Alacsony (csak amit futtat) |
| **Pontosság** | Alacsonyabb (sok hamis pozitív) | Magas (tényleges hibákat talál) |
| **Időzítés** | Korán elkezdhető | Csak futtatható kódnál |
| **Konklúzió** | **Kombináljuk a kettőt!** | |

---

## 4. DevSecOps: Biztonság a CI/CD-ben
A modern fejlesztés során a biztonsági teszteket beépítik az automatizált folyamatba (**Pipeline**).

* **SCA (Software Composition Analysis):** A függőségi fa (külső könyvtárak) átvizsgálása ismert sebezhetőségek (CVE) után . Itt készül az **SBOM** (Software Bill of Materials).
* **Konténer szkennelés:** Docker image-ek vizsgálata elavult OS-összetevők vagy hibás konfigurációk után .
* **Secret Vaults:** Titkos kulcsok, jelszavak kiszűrése a kódból.


---

## 5. Élő rendszerek tesztelése
Amikor a rendszer már (majdnem) éles, jöhetnek a komplexebb vizsgálatok:

1.  **Penetrációs tesztelés (Pentest):** Tényleges támadás szimulálása kontrollált körülmények között .
    * **Fázisai:** Tervezés (scope meghatározása) → Felfedezés (interfészek, CVE-k keresése) → Támadás (kihasználás) → Riport készítés .
2.  **Red Teaming:** Átfogóbb, mint a pentest; a szervezet védelmi képességeit (észlelés, reakció) is teszteli.
3.  **Bug Bounty:** Külsős kutatóknak fizetnek a felfedezett hibákért.

---

**Vizsga-tipp:** Kérdezhetik a **CWE** és **CVE** közötti különbséget. A *CWE* a hiba típusa (pl. "Puffertúlcsordulás"), a *CVE* pedig egy konkrét szoftver konkrét hibája (pl. "A Windows 10 ezen verziójában van egy hiba").

Mehetünk tovább a következő anyagra?