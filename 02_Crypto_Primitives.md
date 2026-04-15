Szia! BME-s diákként tudom, hogy az IT biztonság (VIHIAC07) zh-k nem a felületességről híresek, úgyhogy nézzük is át részletesen a **02-Crypto-Primitives** diasort. Ez az anyag a kriptográfiai építőelemekről (primitívekről) szól, amikre minden későbbi protokoll épül.

Íme a részletes jegyzet:

---

# IT biztonság: Kriptográfiai alapalgoritmusok (Jegyzet)

## 1. Mik azok a kriptográfiai primitívek?
A kriptográfiai primitívek olyan alapvető, jól definiált tulajdonságokkal rendelkező algoritmusok, amelyeket **építőelemként** használunk bonyolultabb protokollok (pl. TLS, SSH) vagy rendszerek tervezésekor.

**Főbb típusok:** 
* **Titkosítási sémák:** Adatbizalmasság biztosítására.
* **Hash-függvények:** Adatintegritás és ujjlenyomat-képzés.
* **MAC-függvények:** Üzenethitelesítés (közös titkos kulccsal).
* **Digitális aláírás:** Üzenethitelesítés és letagadhatatlanság (nyilvános kulccsal).
* **KEM (Key Encapsulation):** Kulcs-beágyazás közös titok létrehozásához.
* **KDF (Key Derivation):** Kulcsszármaztatás meglévő titkokból.
* **PRNG (Pseudo-Random Number Generator):** Álvéletlen-generátorok.

A gyakorlatban ezeket **kriptográfiai könyvtárakon** keresztül érjük el (pl. OpenSSL, PyCryptodome, mbed TLS), amik az algoritmusokon túl kezelik a paddingot (kitöltést), a kulcsformátumokat és a hibajavítást is.

---

## 2. Adatreprezentáció
A kripto-algoritmusok alacsony szinten mindent **byte-sztringként** kezelnek.
* **Hexadecimális írásmód:** A bájtokat gyakran hexapárokkal jelöljük (pl. `93 5A 17`).
* **Szövegkódolás:** * **ASCII:** 7 bites kódolás, 128 karakter (angol ábécé, számok, vezérlőjelek).
    * **UTF-8:** Változó hosszúságú Unicode kódolás. Az ASCII karakterek 1 bájton maradnak, a magyar ékezetes betűk (latin kiterjesztés) általában 2, az emojik 4 bájtot foglalnak.

---

## 3. Titkosítási sémák (Rejtjelezők)
A cél, hogy a küldő és fogadó közötti csatornát lehallgató támadó ne lássa az eredeti üzenetet.

### Csoportosítás kulcs szerint:
1.  **Szimmetrikus (konvencionális):** Ugyanaz a titkos kulcs kell a kódoláshoz és dekódoláshoz.
2.  **Aszimmetrikus (publikus kulcsú):** Külön kódoló (nyilvános) és dekódoló (privát) kulcs van.

### Szimmetrikus típusok:
* **Kulcsfolyam-rejtjelezők (Stream Ciphers):** Bitről-bitre vagy bájtról-bájtra haladnak (pl. Salsa20).
* **Blokkrejtjelezők (Block Ciphers):** Fix méretű blokkokat titkosítanak egyszerre (pl. AES).

---

## 4. XOR művelet és Kulcsfolyam-rejtjelezők
A modern szimmetrikus kriptográfia alapja a **kizáró vagy (XOR)**.
* **Szabály:** $0 \oplus 0 = 0$, $0 \oplus 1 = 1$, $1 \oplus 0 = 1$, $1 \oplus 1 = 0$.
* **Tulajdonság:** $X \oplus X = 0$ és $X \oplus 0 = X$.
* **Inverz:** Ha $X \oplus Y = Z$, akkor $X \oplus Z = Y$.

### Hogyan működik a kulcsfolyam-rejtjelező?
1.  Egy **G** generátor a **seed**-ből (kulcs + nonce) egy tetszőleges hosszú **álvéletlen kulcsfolyamot** ($z_1, z_2...$) gyárt.
2.  **Titkosítás:** $c_i = m_i \oplus z_i$ (üzenet XOR kulcsfolyam).
3.  **Visszafejtés:** $m_i = c_i \oplus z_i$ (ugyanazzal a kulcsfolyammal).

> **Fontos:** Soha ne használd ugyanazt a seedet két üzenethez! A **nonce** (number used once) biztosítja, hogy minden üzenethez egyedi kulcsfolyam szülessen.

---

## 5. Blokkrejtjelezők és Üzemmódok
A blokkrejtjelező fix méretű (pl. AES esetén 16 bájt / 128 bit) blokkokat dolgoz fel.
* **Lavina-hatás (Avalanche effect):** Ha a bemenetben 1 bit megváltozik, a kimenetben a bitek kb. fele (50%) megváltozik kiszámíthatatlanul.

### Miért kellenek módok?
Ha egyforma blokkokat ugyanazzal a kulccsal titkosítunk (ezt hívják **ECB** módnak), a mintázatok láthatóak maradnak a titkosított szövegben is (lásd: a PDF-ben a "titkosított" Linux pingvin képe).

**Alapvető üzemmódok:**
1.  **CBC (Cipher Block Chaining):** Minden nyílt blokkot XOR-olunk az előző titkosított blokkal a titkosítás előtt. Az első blokkhoz egy **IV** (Initialization Vector) kell.
    * *IV követelmény:* Nem kell titkosnak lennie, de **megjósolhatatlannak** igen! 
2.  **CTR (Counter Mode):** A blokkrejtjelezőt kulcsfolyam-generátorként használjuk. Egy számlálót titkosítunk, majd ezt XOR-oljuk az üzenethez. **Párhuzamosítható!** 

**Padding (Kitöltés):** Ha az üzenet nem blokkméret többszöröse, ki kell egészíteni. Mivel a vevőnek tudnia kell, mi az üzenet része és mi a kitöltés, akkor is adunk hozzá egy teljes blokk paddingot, ha amúgy pontosan kijönne a méret.

---

## 6. Aszimmetrikus (Nyilvános kulcsú) kriptográfia
Fő elv: A kódoló és dekódoló kulcs különbözik.
* **Publikus kulcs ($K^+$):** Mindenki ismeri, ezzel titkosítunk.
* **Privát kulcs ($K^-$):** Csak a tulajdonos ismeri, ezzel fejtünk vissza.
* **Biztonság alapja:** Nehéz matematikai problémák (pl. prímfaktorizáció).

### RSA algoritmus (tankönyvi):
* **Generálás:** Válassz két nagy prímet ($p, q$), legyen $n = p \cdot q$. A publikus kitevő $e$, a privát $d$ (az $e$ inverze modulo $\phi(n)$).
* **Titkosítás:** $c = m^e \pmod n$.
* **Dekódolás:** $m = c^d \pmod n$.

> **Figyelem:** A tankönyvi RSA nem biztonságos önmagában, a gyakorlatban speciális formázás (padding) kell hozzá (PKCS#1). Kvantumszámítógéppel feltörhető.

---

## 7. Kriptográfiai hash-függvények
Tetszőleges hosszúságú bemenetet fix hosszúságú (pl. 256 bit) "lenyomattá" képeznek le.
**Hármas követelmény:** 
1.  **Pre-image resistance (Egyirányúság):** Adott hash-ből ne lehessen visszakapni az üzenetet.
2.  **2nd pre-image resistance:** Adott $x$-hez ne lehessen másik $x'$-et találni, aminek ugyanaz a hash-e.
3.  **Collision resistance (Ütközés-ellenállóság):** Ne lehessen találni két tetszőleges üzenetet, amiknek ugyanaz a hash-e.

**Születésnapi paradoxon:** Míg az őskép keresése $O(2^n)$ művelet, ütközést találni sokkal könnyebb: mindössze $O(2^{n/2})$ művelet! 

---

## 8. MAC és Digitális aláírás (Hitelesítés)
### MAC (Message Authentication Code):
* **Működés:** Üzenet + **közös titkos kulcs** → fix hosszúságú kód.
* **Garancia:** Integritás + Eredet-hitelesség (csak az generálhatta, aki tudja a kulcsot).
* **Példa:** HMAC (hash-alapú MAC).

### Digitális aláírás:
* **Működés:** Aszimmetrikus. Aláírás a **privát kulccsal**, ellenőrzés a **publikus kulccsal**.
* **Extra garancia:** **Letagadhatatlanság** (harmadik fél előtt is bizonyítható az eredet).
* **Hash-and-sign paradigma:** Az aláírás lassú, ezért nem az egész üzenetet, hanem csak annak a hash-ét írjuk alá.

---

## 9. Hibrid rendszerek és KDF
### Hibrid titkosítás:
Mivel az RSA lassú, az üzenetet nem azzal titkosítjuk. Alice generál egy **véletlen szimmetrikus kulcsot ($K$)**, ezzel titkosítja az üzenetet (pl. AES-sel), majd csak a $K$-t titkosítja le Bob publikus kulcsával.

### KDF (Key Derivation Function):
Arra szolgál, hogy egy meglévő titokból (pl. jelszóból) biztonságos kulcsot gyártsunk.
* **Jelszavaknál:** Használnak **stretching**-et (szándékos lassítás a brute-force ellen) és **salting**-ot (véletlen érték, hogy ne lehessen előre kiszámolt táblázatokkal támadni).
* **Példák:** PBKDF2, Argon2, scrypt.

---

## 10. Véletlenszám-generálás (PRNG)
A kriptográfiához **megjósolhatatlan** véletlen számok kellenek.
* **Valódi véletlen (TRNG):** Hardveres zajból, lassú (pl. termikus zaj, lávalámpák mozgása).
* **Álvéletlen (PRNG):** Gyors algoritmus, ami egy kis mennyiségű valódi véletlen bemenetből (**seed**) sokat gyárt, ami praktikusan megkülönböztethetetlen a valóditól.
* **Működés:** Gyűjti a környezeti zajt (egérmozgás, disk hozzáférési idő) egy **entrópia pool**-ba, és ebből frissíti a belső állapotát.

---
*Sok sikert a felkészüléshez! Ha jön a következő diasor, küldd bátran.*