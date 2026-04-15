Ez a diasor a kriptográfiai primitívek gyakorlati alkalmazásait mutatja be, a kurzus saját belső rendszereitől kezdve a globális internetes szabványokig, mint a TLS és a PKI.

---

# Kriptográfiai alkalmazások

## 1. Primitívek alkalmazása a tantárgyban
Az oktatók két konkrét példán keresztül szemléltetik a primitívek működését a kurzus során:

### Jelenlétellenőrző tokenek 
A hallgatók jelenlétét egyedi tokenekkel igazolják, amelyeket az **AES blokkrejtjelező** generál.
* **Működés:** Egy 16 bájtos nyílt szöveg blokkot titkosítanak **ECB módban** egy egész félévben használt 32 bájtos titkos kulccsal.
* **A nyílt szöveg (plaintext) felépítése:** Magic sztring ('ac07hu'), dátum (yymmdd), elválasztó ('-') és egy sorszám (pl. '001').
* **Támadás:** Érvényes támadásnak minősül, ha valaki olyan tokent tud generálni, amely a félév kulcsával dekódolva érvényes formátumú nyílt szöveget ad, de nem az oktatók osztották ki.

### Pontszerző előadások kommitmentjei 
Annak bizonyítására, hogy az oktatók nem utólag döntik el, melyik előadás pontszerző, egy **bit-commitment sémát** használnak.
* **Tulajdonságok:**
    * **Binding (kötés):** A közzététel után az érték nem változtatható meg.
    * **Hiding (elrejtés):** A kinyitásig senki nem tudja meghatározni az értéket.
* **Implementáció SHA256-tal:**
    * **Commit fázis:** Generálnak egy 32 bájtos véletlen értéket ($P$). A választott érték ($b$) a $P$ utolsó bitje. Publikálják a $C = \text{SHA256}(P)$ értéket.
    * **Open fázis:** Felfedik $P$-t. Ha az utolsó bit 1, az előadás pontszerző. Bárki ellenőrizheti, hogy a hash megegyezik-e a korábban közzétett $C$-vel.

---

## 2. Nyilvános kulcsú tanúsítványok és PKI 
A nyilvános kulcsok szétosztásánál a legnagyobb kihívás a **hitelesség és integritás** biztosítása (hogy a kulcs valóban azé-e, akinek állítja magát).

### A tanúsítvány fogalma 
A tanúsítvány egy nyilvános kulcs és egy azonosító (név, ID) elválaszthatatlan összekötése egy bizalmi szolgáltató (**Certification Authority – CA**) digitális aláírása által.
* **Tartalma:** A gazda neve, a nyilvános kulcs, a kiállító CA neve és a CA digitális aláírása.

### PKI hierarchia és láncok 
A CA-k hierarchikus fába szerveződnek.
* **Root CA:** A hierarchia csúcsa. Saját kulcsát **önaláírt (self-signed)** tanúsítvánnyal igazolja, amit a felhasználóknak "out-of-band" (pl. az operációs rendszerbe építve) kell megszerezniük.
* **Tanúsítvány-lánc:** Egy entitás hitelességét úgy ellenőrizzük, hogy a láncon végighaladunk a Root CA-ig, minden szinten ellenőrizve a felette lévő aláírását.

### Visszavonás (Revocation) 
Ha egy privát kulcs kompromittálódik vagy az adatok megváltoznak, a tanúsítványt lejárata előtt vissza kell vonni.
* **CRL (Certificate Revocation List):** A CA által rendszeresen közzétett lista a visszavont tanúsítványokról.
* **OCSP:** Online protokoll a visszavonási állapot azonnali lekérdezésére.

---

## 3. TLS (Transport Layer Security) 
A TLS biztonságos csatornát biztosít két távoli fél között (pl. böngésző és webszerver között a HTTPS alapja).

### Alprotokollok (v1.2):
* **Handshake Protocol:** Algoritmusok egyeztetése, felek hitelesítése tanúsítványokkal és közös titok létrehozása.
* **Record Protocol:** Az adatok fragmentálása, titkosítása és integritásvédelme (MAC) a létrehozott kulcsokkal.
* **Alert Protocol:** Hibaüzenetek és figyelmeztetések kezelése.

### Kulcscsere módszerek:
* **RSA alapú:** A kliens a szerver publikus kulcsával titkosítva küld egy titkot.
* **Ephemeral Diffie-Hellman (DHE):** Mindkét fél egyszeri paramétereket használ, ami **Forward Secrecy**-t biztosít (ha a szerver privát kulcsa később kompromittálódik, a korábbi forgalom nem fejthető vissza).

---

## 4. Jellemző hibák a gyakorlatban 
A rendszerek ritkán a kriptográfiai primitívek elméleti gyengesége miatt buknak el; a hiba általában az implementációban vagy a használatban van.

### Híres példák:
* **Verzió-rollback:** Támadó kényszerítheti a feleket egy régebbi, gyengébb protokollverzió (pl. SSL 2.0) használatára.
* **HeartBleed (OpenSSL):** Bemeneti validációs hiba miatt a támadó kiolvashatta a szerver memóriáját (benne privát kulcsokkal, jelszavakkal).
* **Gyenge PRNG (Debian/OpenSSL):** Egy sor kód törlése miatt a véletlenszám-generátor csak a process ID-től függött, így a "véletlen" kulcsok kitalálhatóvá váltak.
* **Side-channel támadások:** Az algoritmus fizikai jellemzőiből (pl. energiafogyasztás vagy végrehajtási idő változása a kulcs bitjeitől függően) következtetnek a titkos kulcsra.

### Összefoglaló tanács
Soha ne használj saját fejlesztésű "kriptográfiai" algoritmust, és figyelj a kulcsok biztonságos generálására (megfelelő entrópia pool használatával).