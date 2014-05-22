#asterisk-disa-callback

Asterisk DISA Callback

#EN:
It is a small Asterisk config file collection to get DISA dialtone with callback.
This can be used to call for (double) VoIP rate instead of the outrageously expensive GSM provider rate.

**_TO USE THIS TUTORIAL YOU WILL NEED TO INSTALL ASTERISK PBX ON YOUR COMPUTER AND AT LEAST 2 OR 3 SIP ACCOUNT_**

##How does it work?

You can request a callback by ringing a trigger number.
You call the trigger number, the PBX does NOT answer your call but send back a busy signal to your phone, thus this call is for free for you and this number can be called from abroad or a phone that has low balance.

After sending a busy signal, the Asterisk will call back your number.

For security reasons, it will call back only the allowed numbers, thus you don't need password to use this system.

When you answer the callback from your PBX, you will get a dialtone then you can type the number you would like to call (in international format, replace + to 00), and finish by pressing the **#** button.

Then it will call the destination number and finally join you.

##How to use this tutorial:

Explanation of config file sections:

###extensions.conf
**[disa-request]**

The callback trigger call will arrive to this context.

**&lt;trigger_number&gt;** The number that receive the trigger call. (Example: 0621380....)
**&lt;allowed_number&gt;** The number that allowed to request callback. You can duplicate this line as much number as you want. (Example: 0036209876543)

**[outbound]**

The number from DISA dialtone context will be called through this context.

**&lt;outbound_callerid&gt;** It will be set as caller ID for your outgoing call. (Example: 0036201234567)

### sip.conf

Default config file, that also includes some more config files.

### sip_providers.conf

You'll need to set the SIP accounts that you would like to use in your DISA callback process.

In the default setup **bc_out** is the callback account, **fvd_out** is the outgoing call account.
**NeoPhoneX-in** is the callback trigger number account.

### sip_registers.conf
You will need to configure your callback trigger accounts in this file.


#HU:
Ez egy kis Asterisk konfig fájl gyűjtemény amivel úgynevezett DISA tárcsahangot kaphatsz visszahívással.
Ezt arra tudod használni, hogy (dupla) VoIP percdíjért telefonálj, a felháborítóan magas GSM szolgáltatói percdíj helyett.

**_E LEÍRÁS HASZNÁLATÁHOZ ASTERISKET KELL TELEPÍTENED A GÉPEDRE, VALAMINT SZÜKSÉGED LESZ LEGALÁBB 2 VAGY 3 SIP FIÓKRA_**

##Hogyan működik?

Egy telefonszám felhívásával visszahívást kérhetsz.
Felhívsz egy telefonszámot, a központ nem veszi fel a hívásod, hanem egy foglalt jelzést küld vissza, így nem kerül pénzbe neked, tehát hívhatod akár külföldről, vagy olyan telefonról is aminek alacsony az egyenlege.

A központ miután foglalt jelzést küldött, visszahív téged.

Biztonsági okokból csak az engedélyezett számokat hívja vissza, így nem kell használnod semmiféle jelszót a híváshoz.

Amikor felveszed a központ visszahívását, egy tárcsahangot fogsz kapni. Ezután elkezdheted beütni a hívni kívánt telefonszámot nemzetközi formátumban. (A + jel helyett 00 tárcsázandó) majd miután begépelted a számot, nyomd meg a **#** gombot a hívás indításához).

Ezután a központ felhívja a beütött számot, majd összekapcsol vele.

##Hogyan használd ezt a leírást:

Magyarázat a konfig fájlok szekcióihoz:

###extensions.conf
**[disa-request]**

A visszahívási kérelem ebbe a contextbe fog érkezni.

**&lt;trigger_number&gt;** Erre a számra érkezik a visszahívási kérelem. (Példa: 0621380....)
**&lt;allowed_number&gt;** Az a szám amelyik nek engedélyezni szeretnénk a visszahívási kérést. Ezt a sort a kívánt számban lemásolhatod egymás alá aszerint, hogy hány telefonszámnak szeretnéd engedélyezni a rendszer használatát. (Példa: 0036209876543)

**[outbound]**

A tárcsahangnál beütött telefonszám hívása ezen a contexten keresztül fog megtörténni.

**&lt;outbound_callerid&gt;** Ez a szám lesz beállítva hívóazonosítónak a kimenő híváshoz. (Example: 0036201234567)

### sip.conf

Alap konfig fájl, ami további beállítófájlokat tölt be.

### sip_providers.conf

Itt be kell állítanod a SIP fiókokat amiket használni szeretnél a visszahívási folyamat során.

Alapesetben a **bc_out** az a fiók amelyik visszahív, **fvd_out** pedig a kimenő hívás fiókja.
**NeoPhoneX-in** a visszahívási kérelmet fogadó fiók.

### sip_registers.conf
A visszahívási kérelmet fogadó fiókot ebben a fájlban is be kell állítanod.
