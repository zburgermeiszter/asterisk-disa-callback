# Asterisk DISA with Callback # 

Call anyone for cheap VoIP rates with a callback.

## Background

In some countries the domestic mobile call rates are so high that it worth to place 2 VoIP calls to talk to someone.  
This project is primarily built to be used in Hungary, but it can be used in other countries as well.  
*(Please note: it is using some local rules to convert domestic numbers to international. Check them before using in other countries.)*  

## Technical implementation

At the moment this project is developed for Asterisk 11 in Docker, 
though it is probably compatible with Asterisk 11 on any other platforms including ARM 
(routers, NAS devices, Raspberry Pi, etc).

## How it works?

There are predefined *trigger numbers* that receives a call from users. The system always send a `busy` signal to the caller,
then check if the caller number is allowed to use the system.

If the caller not allowed the system simply stops the further processing of the request.  
If the caller is allowed to use the system, then rings him back.

After the caller answers the callback, the system checks the type of the original callback request which can be the following:

  - Direct callback
  - Callback with DISA menu

### Direct callback

Direct callback is the case when the system match a callee with a *trigger number*, then dial the caller without any 
further inputs.  
This feature can be used for mapping real phone numbers to virtual numbers to trigger callbacks that connects straight 
to callee without requesting any further inputs from the caller.  

It is a benefit because the *trigger number* now **can be saved to the phonebook:**  
This is useful for very young or elderly people who do not want or can not remember and enter quickdial codes after a callback.  
It is also very convenient for anyone else, because it simply requires a new number to be saved to their phonebook without requiring
the user to change his calling habits too much.

### Callback with DISA menu

DISA is a service that accept a single-digit quickdial number to call a predefined callee, or accept a regular phone number to be dialed.  

## How to use

### Configuration

Find the raw config files in `asterisk11/etc/asterisk`.

### Run

Simply run it with `docker-compose up`.

## Credits

Huge thanks goes to everyone in the hungarian Prohardver.hu community who showed this is a real need in Hungary and encouraged me to start this project.
The [VoIP deep water topic (hugarian)](http://prohardver.hu/tema/voip_melyviz/friss.html) is mostly all about this project.

Huge thanks goes to the following forum members on the Prohardver.hu forum:  
[fujifilm1](http://prohardver.hu/tag/fujifilm1.html) who really encourage me to keep developing new features for this 
project and always give a helping hand in testing, trying out new features.  
[HXY](http://prohardver.hu/tag/hxy.html) who kicked me to implement the Direct Callback feature which was missing from the project for so long.  
[Domestos](http://prohardver.hu/tag/domestos.html) who helped to improve the project with making the dialplan more 
secure and cleaned up the code a bit.

Huge thanks to the developers of the [Asterisk PBX](http://www.asterisk.org/), [voip-info.org](http://www.voip-info.org/),
Google, my first employer in Hungary that introduced the world of Asterisk to me, Dellmont for cheap VoIP rates, 
[NeoPhone](http://web.neophone.hu/) for hungarian virtual numbers.