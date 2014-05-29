#asterisk-disa-callback

Asterisk DISA Callback

#HU:
Ez egy kis Asterisk konfig fájl gyűjtemény amivel úgynevezett DISA tárcsahangot kaphatsz visszahívással.
Ezt arra tudod használni, hogy (dupla) VoIP percdíjért telefonálj, a felháborítóan magas GSM szolgáltatói percdíj helyett.

**_E LEÍRÁS HASZNÁLATÁHOZ ASTERISKET KELL TELEPÍTENED A GÉPEDRE, VALAMINT SZÜKSÉGED LESZ LEGALÁBB 2 VAGY 3 SIP FIÓKRA_**

##Hogyan működik?

Egy telefonszám felhívásával visszahívást kérhetsz.
Felhívsz egy telefonszámot, a központ nem veszi fel a hívásod, hanem egy foglalt jelzést küld vissza, így nem kerül pénzbe neked, tehát hívhatod akár külföldről, vagy olyan telefonról is aminek alacsony az egyenlege.

A központ miután foglalt jelzést küldött, visszahív téged.

Biztonsági okokból csak az engedélyezett számokat hívja vissza, így nem kell használnod semmiféle jelszót a híváshoz.

Amikor felveszed a központ visszahívását, egy tárcsahangot fogsz kapni. 

Ekkor ha 1-9 ig nyomsz gombot, akkor a gyorshívásra betárolt számokat fogja hívni, ha olyat választottál ami üres memóriahely, akkor marad a választómenüben és újat próbálhatsz, ha 0-t nyomsz, akkor nem betárolt telefoszámot hívhatsz.

Ha 0t nyomtál elkezdheted beütni a hívni kívánt telefonszámot nemzetközi formátumban. (A + jel helyett 00 tárcsázandó) majd miután begépelted a számot, nyomd meg a **#** gombot a hívás indításához).

Ezután a központ felhívja a beütött számot, majd összekapcsol vele.

A a kimenő hívás hívóazonosítója a visszahívást fogadó telefonszám lesz. Tehát olyan, mintha a saját számodról hívnád. Ezen funkció helyes működéséhez ne felejtsd el hozzáadni a (FVD/BC) webes felületen a telefonszámot, mert csak azokat a telefoszámokat tudja kijelezni a rendszer amik meg lettek erősítve korábban.

##Hogyan használd ezt a leírást:

Magyarázat a konfig fájlok szekcióihoz:

###extensions_global.conf

Ez a fájl tartalmazza a dialplan által használt telefonszámokat és más paramétereket.

**ALLOWED_NUMBER_0**<br>
Az első telefonszám amelyiknek engedélyezni szeretnénk a visszahívási kérést. Ezt a sort a kívánt számban lemásolhatod egymás alá aszerint, hogy hány telefonszámnak szeretnéd engedélyezni a rendszer használatát. (Példa: 0036209876543)
Amennyiben több telefonszámnak szeretnéd engedélyeznia rendszer használatát változtatnod kell a változó nevében szereplő számot, (0..1..2..3), illetve az extensions.conf ban is le kell másolnod a megfelelő sorokat, és az új változóneveket kell használnod.

**RECORD_CALL**<br>
Hívásrögzítés bekapcsolása.<br>
Be: **ON**<br>
Ki: **OFF**

**RECORD_FORMAT**<br>
Hívásrögzítési formátum.<br>
Alapértelmezett **wav**.

**RECORD_PATH**<br>
Hívásrögzítési útvonal. <br>
Alapértelmezett: **/var/spool/asterisk/monitor/**

**TRIGGER_NUMBER**<br>
Erre a számra érkezik a visszahívási kérelem. (Példa: 0621380....)

**QUICKDIAL_1 .. 9**<br>
Gyorshívás gombok telefonszámai 1-9

###extensions.conf
**[disa-request]**<br>
A visszahívási kérelem ebbe a contextbe fog érkezni.

**[disa-out]**<br>
Ez a context kéri be a hívandó telefonszámot. Jelenleg ez a context tárcsahangot ad.

**[outbound]**<br>
A tárcsahangnál beütött telefonszám hívása ezen a contexten keresztül fog megtörténni.

**[hangup-context]**<br>
Ennek a contextnek jelenleg csupán a hívásmegszakítás a célja.

### sip.conf

Alap konfig fájl, ami további beállítófájlokat tölt be.

### sip_providers.conf

Itt be kell állítanod a SIP fiókokat amiket használni szeretnél a visszahívási folyamat során.

Alapesetben a **bc_out** az a fiók amelyik visszahív, **fvd_out** pedig a kimenő hívás fiókja.
**NeoPhoneX-in** a visszahívási kérelmet fogadó fiók.

### sip_registers.conf
A visszahívási kérelmet fogadó fiókot ebben a fájlban is be kell állítanod.


##Hardverplatform konfigurációk
Erre a részre akkor lesz szükséged, ha valamilyen hardveren szeretnéd használni a konfig csomagot.

### TP-LINK TL-WR1043ND (OpenWRT)
A TP-LINK TL-WR1043ND router egy igen népszerű router manapság. OpenWRT firmware használatával felteleptheted rá az Asterisk alközpontot. Ahhoz, hogy a mellékelt konfigfájlok megfelelően működjenek, az alábbi csomagokat érdemes teleptened (11.8.1-1 verzióval tesztelve):

- asterisk11
- asterisk11-app-originate
- asterisk11-app-read
- asterisk11-app-system
- asterisk11-codec-alaw
- asterisk11-codec-gsm
- asterisk11-codec-ulaw
- asterisk11-func-groupcount

Ezeket a csomagokat egy úgynevezett csomagtárolóból tudod letölteni, ehhez nyisd meg a ** System &gt; Software &gt; Configuration** panelt, és szúrd be a következő sort a szövegmező elejére:
**src/gz snapshots http://downloads.openwrt.org/snapshots/trunk/ar71xx/packages/**
