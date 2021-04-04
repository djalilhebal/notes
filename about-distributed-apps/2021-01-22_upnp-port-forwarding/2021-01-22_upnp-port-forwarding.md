---
date: "2021-01-22"
permalink: "/upnp-automatic-port-forwarding/"
...

# Automatic port forwarding using UPnP
#Networking #ModulePOC-related.

In one word: UPnP.

If you can torrent with no issue (specifically, being able to upload to peers) then this option should just work.

It uses SOAP (think: XML-RPC), which is conceptually not unlike RMI.
<aside>
  (Just saying, JSON-RPC is cuter and gPRC is better.)
</aside>

This reminds me of that dead meme (off topic, but also see `[stackexchange-ize-vs-ify]`):
![meme: "Yo dawg I head you like POC so we POC'ified yo POC TP so you can RPC while you RPC"][my-meme--yo-dawg-poc-rpc]


## GUI app to open ports

There's this open source project called PortMapper by GitHub user **kaklakariada** <sup>`[portmapper]`</sup>.

The screenshot below shows that I've <em>Use</em>d/applied my "POC/JavaRMI" preset to open ports 25000 to 25099 (a reference to ___University of Constantine___ (25) ___2___?).

KAITO: 192.168.1._**2**_**39** is my _**Secondary**_ (2) **ThankYou** (39/SanKyu) server.

KAITO: Notice that qBittorrent has automatically configured port forwarding for Host No. 4 ("Unhappy").  
If we wanted, we could check the source code of either qBittorrent or PortMapper to see how they work,  
and possibly to automate this process from within our Java RMI project -- imagine: When we first launch the "server," check if our RMI `Registry` and exported `Remote` objects' ports are accessible to external devices; else, open them.  
...And this is one of the reasons why we use open-source stuff.

![PortMapper showing my "POC/JavaRMI" preset][portmapper--poc-javarmi-preset]

![Wireshark captured a UPnP response message that enabled port forwarding for port 25001 to the Secondary ThankYou server][wireshark--upnp-enabled-port-25001]

Tested with:
- [`@kaklakariada/portmapper v2.1.1`](https://github.com/kaklakariada/portmapper/releases/tag/v2.1.1)
- `OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_272-b10)`
- Windows 10 x64.
- Some commonplace D-Link modem (UPnP is enabled by default).


## Testing

### Simple webserver
Python (Spyder 3) and Java (JDK 8) were all I had on my Windows installation.

I used Python to start an HTTP server that's listening on port 25001 (I could have chosen any value from the range \[25000-25099\]).

```py
# -*- coding: utf-8 -*-
"""
SEE: https://docs.python.org/3/library/http.server.html
"""

import os
from http.server import HTTPServer, CGIHTTPRequestHandler

os.chdir('.')
server_object = HTTPServer(
    server_address=('', 25001),
    RequestHandlerClass=CGIHTTPRequestHandler
    )
server_object.serve_forever()
```

![Spyder running a simple HTTP server on port 25001][spyder--httpserver-port-25001]

![Google Chrome browsing the localhost web server on port 25001][chrome--localhost-port-25001]

### Can others see me now?
Tested using `CanYouSeeMe.org`<sup>`[canyouseeme]`</sup>.
TCP port 25001 can be seen? **Yes**.

(Instead of that website, I could've used Brave's Tor feature or some VPN --like ProtonVPN--, or even my mobile connection to test whether these ports were externally accessible~)

((KAITO: That Tor approach is what I've done in Attendz, our #SmartUniversity shit.))

((KAITO: Did I just expose my external IP address? Well, "_IPs just come and go like seasons, dynamilicious~_".))

![CanYouSeeMe.org reports that it can see my 25001 port][chrome--canyouseeme-port-25001]


## References

- `[portmapper]` [`@kaklakariada/portmapper`](https://github.com/kaklakariada/portmapper)
  * "A tool for managing port forwardings via UPnP"
  * In Java.

- `[canyouseeme]` [CanYouSeeMe.org: _Open Port Check Tool_](https://canyouseeme.org)
  * NOTE: I've chosen this website randomly.

- [What is UPnP and why you should disable it immediately | NordVPN](https://nordvpn.com/blog/what-is-upnp/) `[interesting][unarchivable:20210122]`

- `[stackexchange-ize-vs-ify]` [What is the difference between the suffixes -ize and -ify? | English - Stack Exchange](https://english.stackexchange.com/questions/189530/what-is-the-difference-between-the-suffixes-ize-and-ify) `[interesting] #English`

- See also:

  * [wiki:SOAP](https://en.wikipedia.org/wiki/SOAP)

  * [wiki:Universal Plug and Play](https://en.wikipedia.org/wiki/Universal_Plug_and_Play) (UPnP)

  * [wiki:Simple Service Discovery Protocol](https://en.wikipedia.org/wiki/Simple_Service_Discovery_Protocol) (SSDP)

  * [wiki:Internet Gateway Device Protocol](https://en.wikipedia.org/wiki/Internet_Gateway_Device_Protocol) (IGD)


<!-- IMAGES: GITHUB
[my-meme--yo-dawg-poc-rpc]: https://user-images.githubusercontent.com/32184973/105508390-994d5d00-5ccc-11eb-8a64-861b60382d9a.jpg

[chrome--canyouseeme-port-25001]: https://user-images.githubusercontent.com/32184973/105508380-96eb0300-5ccc-11eb-9340-2a5f708cde98.png
[chrome--localhost-port-25001]: https://user-images.githubusercontent.com/32184973/105508388-98b4c680-5ccc-11eb-88df-d98c2913c89d.png
[portmapper--poc-javarmi-preset]: https://user-images.githubusercontent.com/32184973/105508393-99e5f380-5ccc-11eb-94df-43d11454fedb.png
[spyder--httpserver-port-25001]: https://user-images.githubusercontent.com/32184973/105508398-9b172080-5ccc-11eb-8f9c-81d5865e2e4f.png
[wireshark--upnp-enabled-port-25001]: https://user-images.githubusercontent.com/32184973/105508403-9bafb700-5ccc-11eb-81be-5b1b70b23fe4.png
-->

<!-- IMAGES: LOCAL -->
[my-meme--yo-dawg-poc-rpc]: ./my-meme--yo-dawg-poc-rpc.jpg

[chrome--canyouseeme-port-25001]: ./screenshots/chrome--canyouseeme-port-25001.png
[chrome--localhost-port-25001]: ./screenshots/chrome--localhost-port-25001.png
[portmapper--poc-javarmi-preset]: ./screenshots/portmapper--poc-javarmi-preset.png
[spyder--httpserver-port-25001]: ./screenshots/spyder--httpserver-port-25001.png
[wireshark--upnp-enabled-port-25001]: ./screenshots/wireshark--upnp-enabled-port-25001.png
