Szia! Benne vagyok, vágjunk bele a felkészülésbe. Ez az első diasor az **implementációs hibákról** szól, ami az IT biztonság egyik legalapvetőbb témaköre. Különösen a memóriakezelési hibákra fókuszálunk, ami a C/C++ világában a "kenyér és vaj" a támadók számára.

Íme a részletes, strukturált jegyzet a diasor alapján:

---

# IT Biztonság: Implementációs hibák jegyzet

## 1. Szoftverbiztonság alapfogalmak
A szoftver akkor tekinthető **biztonságosnak**, ha teljesíti a meghatározott vagy feltételezett biztonsági célokat. Fontos megjegyezni, hogy a biztonsági követelmények gyakran **konfliktusba kerülnek** egymással:
* **Példa:** Ha egy támadás észlelésekor zároljuk a rendszert, az védi a *titkosságot* és az *integritást*, de sérti a *rendelkezésre állást*.

### Sérülékenységi kategóriák:
1.  **Memóriakezelési hibák:** Térbeli (pl. túlindexelés) és időbeli (pl. felszabadított memória használata).
2.  **Strukturált kimenet-generálás:** SQL injection, XSS, Command injection.
3.  **Versenyhelyzet (Race Condition):** Pl. TOCTOU (Time of Check to Time of Use).
4.  **API sérülékenységek:** Az implementációs hibák általánosítása "szerződésszegésként".
5.  **Side-channel (Oldalcsatorna):** Jellemzően kriptográfiai probléma (pl. futásidőből vagy áramfelvételből következtetni a titkos kulcsra).

---

## 2. Memória-korrupció és Puffertúlcsordulás
A **memória-korrupció** akkor következik be, ha egy memóriacím tartalma nem szándékos módon (programozási hiba miatt) módosul. Ez összeomláshoz vagy definiálatlan viselkedéshez vezethet.

### Típusai :
* **Buffer overflow:** Stacken vagy heapen.
* **Integer overflow:** Számábrázolási hiba, ami később hibás memóriafoglaláshoz vezethet.
* **Pointer hibák:** NULL pointer dereferálás vagy dangling pointer (már felszabadított területre mutat).

### Puffertúlcsordulás (Buffer Overflow) alapelve:
Akkor fordul elő, ha több adatot írunk egy pufferbe, mint amennyi helyet foglaltunk neki. A támadó ezzel felülírhatja a kritikus adatokat:
* Változó értékeket.
* Pointereket.
* **Visszatérési címeket (Return address).**
Ez lehetővé teszi a program vezérlési folyamatának megváltoztatását és rosszindulatú kód végrehajtását .

---

## 3. x86 Architektúra és Stack működése
A támadások megértéséhez ismernünk kell az alapokat.

### Regiszterek (32 bites példák) :
* **EIP (Instruction Pointer):** A következő végrehajtandó utasítás címe. **Ez a támadó fő célpontja.**
* **ESP (Stack Pointer):** A stack tetejére mutat.
* **EBP (Base/Frame Pointer):** Az aktuális stack frame közepére/aljára mutat. Segít elérni a paramétereket és lokális változókat.
* **EAX, EBX, ECX, EDX:** Általános célú regiszterek.

### A Stack Frame (Függvénykeret) felépítése:
A memóriában "lefelé" (a magasabb címek felől az alacsonyabbak felé) növekszik a stack. Egy függvényhíváskor így néz ki a stack tartalma (felülről lefelé, azaz növekvő memóriacímek szerint):
1.  **Lokális változók** (EBP-nél kisebb címeken).
2.  **Elmentett EBP regiszter** (SEBP).
3.  **Visszatérési cím (Return Address)** (EBP + 4 címen).
4.  **Függvény paraméterek** (EBP + 8, +12, stb. címeken).


---

## 4. A Stack Overflow támadás menete
A támadó egy olyan függvényt keres, amely méretellenőrzés nélkül másol adatot a stackre (pl. `strcpy`, `gets`, `sprintf`).

### Lépések :
1.  A támadó egy speciálisan összeállított (túl hosszú) bemenetet ad a programnak.
2.  A másolás során az adat "kifolyik" a lokális pufferből.
3.  Felülírja a mentett EBP-t, majd a **Visszatérési címet**.
4.  Amikor a függvény véget ér (`RET` utasítás), a CPU a támadó által megadott (módosított) címre ugrik ahelyett, hogy visszatérne a hívóhoz.

### Shellkód és NOP sled:
* **Shellkód:** Egy apró, gépi kódú program, amit a támadó le akar futtatni (pl. elindít egy parancssort).
* **NOP sled:** `0x90` (No Operation) utasítások sorozata a shellkód előtt. Ha a támadó nem tudja pontosan a shellkód címét, elég a NOP sorozat közepére ugrania; a CPU végrehajtja a semmit, amíg el nem éri a valódi kódot.
* **0x00 bájtok elkerülése:** A karakterlánc-kezelő függvények (pl. `strcpy`) a null-bájtot a sztring végének tekintik, így a shellkód nem tartalmazhat ilyet, különben a másolás félbeszakadna.

---

## 5. Védelmi mechanizmusok (Mitigációk)
Mivel a programozók hibáznak, a rendszerekbe építettek "kerülőmegoldásokat".

### 1. DEP / NX (Data Execution Prevention / No-eXecute):
* **Alapötlet:** Válasszuk szét az írható és a futtatható memóriaterületeket.
* A stack legyen **írható**, de **NE legyen futtatható**. Így ha a támadó shellkódot is juttat a stackre, nem tudja elindítani, mert a CPU hibát generál.
* **Hardveres támogatás:** NX bit a memórialapokon.

### 2. Stack Canary (Stack kanári):
* **Működése:** A fordító egy titkos értéket (kanárit) helyez el a lokális változók és a visszatérési cím közé.
* A függvény visszatérése előtt ellenőrzi: ha a kanári értéke megváltozott, az azt jelenti, hogy puffertúlcsordulás történt.
* Ilyenkor a program azonnal leáll, megelőzve a támadó kódjának lefutását.

### 3. ASLR (Address Space Layout Randomization):
* **Működése:** Az OS véletlenszerűen választja meg a stack, a könyvtárak (libc) és a program kódjának helyét a memóriában minden indításkor.
* **Hatása:** A támadó nem tudja megjósolni, hova kell ugrania (nem tudja a shellkód vagy a függvények pontos címét).
* **32 bit vs 64 bit:** 32 biten korlátozott (16 bitnyi véletlenszerűség, brute-force-olható), 64 biten viszont nagyon erős (40 bitnyi véletlenszerűség).

### 4. Nyelvi és tesztelési megoldások :
* **Biztonságos nyelvek:** Java (JVM garantálja a memóriabiztonságot), Rust (fordítási időben ellenőrzi a tulajdonosi modellt, kizárja a memória-korrupciót).
* **Sanitizerek:** AddressSanitizer (ASan) – tesztelés során segít felismerni a memóriahibákat.

---

**Tipp a vizsgához:** Figyeld meg a diasorban a történelmi példákat (Morris worm, SQL Slammer) – ezek mutatják meg, mekkora pusztítást tud okozni egyetlen puffertúlcsordulás .

Van még 9 diasorunk, melyikkel folytassuk?