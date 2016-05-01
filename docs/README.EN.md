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
