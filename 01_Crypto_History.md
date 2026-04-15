Szuper, megvan a második anyag! Ez a diasor a kriptográfia történelmére és az alapvető matematikai/elméleti koncepciókra fókuszál. Itt már előkerülnek klasszikus és modern algoritmusok is, valamint a kvantum-veszély, amit imádnak visszakérdezni a vizsgákon.

Lássuk a részleteket, markdown formátumban!

---

## 01. Diasor: Kriptográfia - Történelmi áttekintés

Ez a modul a kriptográfia fejlődését mutatja be az ókori módszerektől a modern, nyilvános kulcsú rendszereken át egészen a kvantumszámítógépek jelentette fenyegetésig és a posztkvantum kriptográfiáig .

---

### 1. Kriptográfiai Alapfogalmak és a Kerckhoff-elv
* **Mire jó a kriptográfia?** Algoritmikus módszereket ad információbiztonsági szolgáltatások megvalósítására.
    * *Titkosítás (rejtjelezés):* Bizalmasságot (confidentiality) ad.
    * *MAC (Message Authentication Code):* Integritást és eredet-hitelesítést ad.
    * *Digitális aláírás:* Integritást, eredet-hitelesítést és **letagadhatatlanságot (non-repudiation)** nyújt.
* **A rejtjelezés modellje:** A küldő egy nyílt szöveget (plaintext) egy kódoló algoritmus és egy **kulcs** segítségével titkosít (ciphertext), amit a fogadó a megfelelő kulccsal dekódol . 
* **A Kerckhoff-elv (1883):** A rendszer biztonsága **nem** alapulhat magának a kódoló algoritmusnak a titkosságán . A rendszernek akkor is biztonságosnak kell maradnia, ha a támadó az algoritmus minden részletét ismeri, egyedül a **kulcsnak** kell titkosnak lennie.
* **Kulcstér (Key space):** Az összes lehetséges kulcs halmaza. Ha ez túl kicsi, a támadó az összes kulcs kipróbálásával feltörheti a rendszert. Ezt hívjuk **kimerítő kulcskeresésnek (brute force attack)** .

---

### 2. Történelmi Rejtjelezők és Törésük
* **Szkütale:** Ókori görög (spártai) mechanikus eszköz, amely **transzpozíciós** (a betűk sorrendjét felcserélő) rejtjelezést valósít meg .
* **Caesar-rejtjelező:** **Mono-alfabetikus helyettesítés** (substitution). Minden betűt az ábécében eltolt (pl. 3 hellyel későbbi) betűvel helyettesít (pl. A $\rightarrow$ D) . Általánosított formájában a kulcs az eltolás mértéke .
* **Gyakoriságanalízis:** Bár egy általános mono-alfabetikus helyettesítés kulcstere hatalmas ($26! \approx 4 \cdot 10^{26}$), a gyakorlatban könnyen feltörhető . Miért? Mert a betűk eltolása nem változtatja meg a betűk **előfordulási gyakoriságának arányát** . Az angol nyelvben pl. az "e" betű a leggyakoribb, így a titkosított szöveg leggyakoribb karaktere jó eséllyel a nyílt "e" betű kódja .

---

### 3. Az Enigma
Az Enigma egy II. világháborús német elektro-mechanikus titkosító gép, amely **poli-alfabetikus helyettesítést** végez (minden betű leütésénél változik a kódábécé, mivel a rotorok elfordulnak) .
* **Komponensei:** Billentyűzet, Lámpa-panel, Plugboard (betűpárok felcserélése), Rotorok (keverés) és Umkehrwalze (tükröző, ami miatt a gép önmaga inverze) .
* **Kulcstér:** A kezdeti beállítás (rotorok típusa, sorrendje, kezdőállása, plugboard beállítások) adja a kulcsot, ami kb. $2^{53}$ méretű .
* **A feltörés története:** 1. A lengyel **Marian Rejewski** a protokoll gyengeségét használta ki: a németek az üzenetkulcsot (kezdeti rotorállást) kétszer ismételve küldték el az üzenet elején a napi beállítással titkosítva. Erre építették a "Bomba" nevű gépet .
    2. Később a brit **Alan Turing** (Bletchley Park) az **ismert nyílt szövegű támadás (known-plaintext attack)** módszerével és a megépített "Bombe" gépekkel törte a megerősített Enigmát . Segítették őket a német operátorok által használt "gyenge kulcsok" (cillies) és a merev protokollszabályok is .

---

### 4. A Modern Kriptográfia Születése
* **Claude E. Shannon (1949):** Megteremtette a rejtjelezés információ-elméleti alapjait .
* **DES (Data Encryption Standard - 1970-es évek):** Az IBM által fejlesztett **szimmetrikus kulcsú** blokkrejtjelező. Jellemzői: Feistel-struktúra, 16 kör (réteg), 64 bites blokkméret, **56 bites kulcs** .
* **Diffie-Hellman Kulcscsere (1976):** Megoldotta a közös kulcs biztonságos elosztásának problémáját a hálózaton keresztül .
    * Alice és Bob megegyeznek egy $g$ és $p$ (prím) nyilvános számban .
    * Alice választ egy titkos $a$-t, elküldi: $A = g^a \bmod p$.
    * Bob választ egy titkos $b$-t, elküldi: $B = g^b \bmod p$ .
    * Közös titok: Alice kiszámolja $K = B^a \bmod p$, Bob kiszámolja $K = A^b \bmod p$. Mindketten a $g^{ab} \bmod p$ értéket kapják . Biztonsága a diszkrét logaritmus probléma nehézségén alapul.
* **RSA (1977):** Rivest, Shamir és Adleman algoritmusa, amely bevezette az **aszimmetrikus (nyilvános kulcsú)** kriptográfiát . Két kulcs van: egy nyilvános (kódoláshoz) és egy privát (dekódoláshoz/aláíráshoz) .
    * Működése (képlet): Titkosítás: $C = M^e \bmod n$, Dekódolás: $M = C^d \bmod n$ .
    * Ahol a privát és publikus kulcs matematikai kapcsolata: $e \cdot d \equiv 1 \pmod{(p-1) \cdot (q-1)}$.
    * Biztonsága a **prímtényezős felbontás (faktorizáció)** nehézségén alapul .
* **PGP (Pretty Good Privacy - 1991):** Phil Zimmermann szoftvere, amely a történelemben először tette elérhetővé az erős kriptográfiát a hétköznapi emberek számára .

---

### 5. A Kvantum-veszély és a Posztkvantum Kriptográfia (PQC)
A kvantumszámítógépek forradalmasítják a számítási kapacitást, ami komoly fenyegetés a mai kriptográfiára .
* **Grover algoritmus (1996):** Négyzetes gyorsítást ad a kimerítő kulcskeresésre . *Hatása:* A szimmetrikus kriptográfia (pl. AES) megmenthető a kulcsméret megduplázásával (pl. 128 bitről 256 bitre).
* **Shor algoritmus (1994):** Exponenciális gyorsítást ad a faktorizációra és a diszkrét logaritmusra . *Hatása:* **Teljesen feltöri a ma használt aszimmetrikus rendszereket** (RSA, DSA, ECDSA). A kulcsméret növelése itt már nem segít.
* **Harvest-now-decrypt-later (Gyűjtsd be most, törd fel később) probléma:** A támadók már most rögzítik a titkosított forgalmat, várva, hogy 10-20 év múlva megépüljön a kvantumszámítógép, amivel majd visszamenőleg dekódolják azt .
* **A megoldás két ága:**
    1. **Posztkvantum Kriptográfia (PQC):** Olyan *új matematikai alapokon* (pl. rács alapú) nyugvó algoritmusok, amelyek **hagyományos számítógépen futnak**, de ellenállnak a kvantumtámadásoknak (NP-nehéz problémákra épülnek) . (Jelenleg a NIST végzi ezek szabványosítását, cél a 2035-ös átállás ).
    2. **Kvantum-kriptográfia (QKD):** Nem matematikai, hanem **fizikai elveken** (kvantum-összefonódás, nincs másolás tétel) alapuló hardveres megoldás közös kulcsok biztonságos megosztására (pl. BB84 protokoll fotonokkal) .

---

### 🛡️ Kritikus pontok (ZH-tippek)
* **PQC vs. QKD:** Szigorúan válaszd külön! A *Posztkvantum kriptográfia (PQC)* szoftveres, matematikai megoldás normál gépekre . A *Kvantum-kriptográfia (QKD)* fizikai (optikai) hardvert igényel.
* **Mi a különbség a szimmetrikus és aszimmetrikus kulcsú rejtjelezés között?** Szimmetrikusnál *ugyanaz* a titkos kulcs kódol és dekódol. Aszimmetrikusnál van egy publikus kulcs a kódoláshoz (vagy aláírás-ellenőrzéshez) és egy titkos privát kulcs a dekódoláshoz (vagy aláíráshoz) .
* **Redukciós bizonyítás:** Azt bizonyítjuk, hogy az algoritmusunk feltörése pontosan annyira nehéz, mint egy ismerten nehéz matematikai probléma (pl. prímtényezős felbontás) megoldása .
* **Kerckhoff-elv:** "A biztonság nem alapulhat az algoritmus titkosságán." - Ezt kívülről fújd, alapvetés! .

Jöhet a 3. diasor is? Hogy hívják a következő fájlt?