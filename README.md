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

A kimenő hívás hívóazonosítója a visszahívást fogadó telefonszám lesz. Tehát olyan, mintha a saját számodról hívnád. Ezen funkció helyes működéséhez ne felejtsd el hozzáadni a (FVD/BC) webes felületen a telefonszámot, mert csak azokat a telefoszámokat tudja kijelezni a rendszer amik meg lettek erősítve korábban.

##Hogyan használd ezt a leírást:

Magyarázat a konfig fájlok szekcióihoz:

###extensions_global.conf

Ez a fájl tartalmazza a dialplan által használt telefonszámokat és más paramétereket.

**ALLOWED_NUMBER_0 .. n**<br>
Aon telefonszám amelyiknek engedélyezni szeretnénk a visszahívási kérést. Ezt a sort a kívánt számban lemásolhatod egymás alá aszerint, hogy hány telefonszámnak szeretnéd engedélyezni a rendszer használatát. (Példa: 0036209876543)
Amennyiben több telefonszámnak szeretnéd engedélyeznia rendszer használatát változtatnod kell a változó nevében szereplő számot, (0..1..2..3), illetve az extensions.conf ban is le kell másolnod a megfelelő sorokat, és az új változóneveket kell használnod.

**ALLOW_INTERNALS_OUTBOUND**<br>
Belső mellékek kimenő irányú hívásainak engedélyezése.
Be: **ON**<br>
Ki: **OFF**

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

**REWRITE_COUNTRY**<br>
Azon országnak a kódja amelyiknek a helyi telefonszám formátumát át szeretnénk írni nemzetközi formátumba hívószámkijelzéshez.
Jelenleg csak a "hu" kód él.
Átalakítja a 06 os számokat 0036 os számokká.
Ha csak 7 számjegyű a hívóazonosító érkezik, eléfűzi a 0621 előtagot.
Ha nem magyar szám érkezik (pl 0044...) akkor nem ír át semmit.
Ez azt jelenti, hogy engedélyezett számnak lehet vegyesen magyar és külföldk számot is megadni.
FONTOS, hogy a visszahíváskérést fogadó telefonszám MAGYAR legyen. 
(Pl valamilyen 21 es körzetű szám)

**TRUNK_CALLBACK**<br>
Annak a SIP fióknak a neve amelyiken a visszahívást szeretnénk indítani.
(A fiók neve a sip_providers.conf ban lévő kategória neve.)

**TRUNK_CALLBACK_MAX_CALLS**<br>
A visszahívást indító SIP fiókon engedélyezett egyidejű hívások száma.

**TRUNK_OUTGOING**<br>
Annak a SIP fióknak a neve amelyiken a kimenő hívást szeretnénk indítani.
(A fiók neve a sip_providers.conf ban lévő kategória neve.)

**TRUNK_OUTGOING_MAX_CALLS**<br>
A kimenő hívást indító SIP fiókon engedélyezett egyidejű hívások száma.

**TRIGGER_NUMBER**<br>
Erre a számra érkezik a visszahívási kérelem. (Példa: 0621380....)
(Jelenleg nem használt)

**QUICKDIAL_1 .. 9**<br>
Gyorshívás gombok telefonszámai 1-9

###extensions.conf
**[disa-request]**<br>
A visszahívási kérelem ebbe a contextbe fog érkezni.
Itt dől el, hogy engedélyezett számról van-e szó, és visszahívható-e?

**[disa-out]**<br>
Ez a context kéri be a gyorshívást, 0 megnyomása esetén pedig a hívandó telefonszámot. 
Jelenleg ez a context tárcsahangot ad.

**[outbound]**<br>
A tárcsahangnál beütött telefonszám hívása ezen a contexten keresztül fog megtörténni.

**[busy]**<br>
Ennek a contextnek a foglalt jelzés küldés a célja.

**[hangup]**<br>
Ennek a contextnek a hívásmegszakítás a célja.

**[hangup-context]**<br>
Jelenleg nem használt.

**[originate-context]**<br>
Ezen a contexten keresztül végezzük a hívószámkijelzéses visszahívás indítását.

**[macro-sip-dialer]**<br>
Ez a makró végez minden kimenő hívásindítást, az alapján, hogy az egyidőben engedélyezett hívások számát túlléptük e.

**[macro-stdvoip]**<br>
Jelenleg nem használt.
(A sip-dialer egyszerűsített verziója)

**[macro-international-format]**<br>
Ez a makró végzi eg a telefonszámok helyi formátumról (06) nemzetközi formátumra (0036) alakítását.

### extensions_internal.conf

A belső mellékek routolását irányító dialplan részlet.


### sip.conf

Alap konfig fájl, ami további beállítófájlokat tölt be.

### sip_clients.conf

Belső mellékek konfigurációs fájlja.<br>
**\[client1\](internal_clients)** Egy mellék szekciója. Kívánt számban lemásolható.<br>
**\[client1\]** \[Felhasználónév\]<br>
**secret ** Jelszó (Lehetőleg random, minimum 16 karakteres értéket állíts be jelszónak.)<br>
**cid_number** A mellék kimenő hívásokhoz küldendő hívóazonosítója.

### sip_providers.conf

Itt be kell állítanod a SIP fiókokat amiket használni szeretnél a hívások során.

Alapesetben a **bc_out** az a fiók amelyik visszahív, **fvd_out** pedig a kimenő hívás fiókja.
**NeoPhoneX-in** a visszahívási kérelmet fogadó fiók.

### sip_registers.conf
A visszahívási kérelmet fogadó fiókot ebben a fájlban is be kell állítanod.
Célszerű IP címet megadni URL helyett egyes eszközökön.

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

Ezeket a csomagokat egy úgynevezett csomagtárolóból tudod letölteni, ehhez nyisd meg a ** System &gt; Software &gt; Configuration** panelt, és szúrd be a következő sort a szövegmező elejére:<br>
**src/gz snapshots http://downloads.openwrt.org/snapshots/trunk/ar71xx/packages/**
