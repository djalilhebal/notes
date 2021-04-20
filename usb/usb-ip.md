# USB/IP
#USB #ModuleSD-related

USB/IP is a protocol used to share USB devices transparently via the network.

---

- [ ] RECHECK: [USB Request Block (URB) — The Linux Kernel documentation](https://www.kernel.org/doc/html/v4.14/driver-api/usb/URB.html)
    * KAITO: To understand and fork _USB/IP Server_.

- [x] CHECK/TRY: [Quick How-To | ArchWiki](https://wiki.archlinux.org/index.php/USB/IP)

- Hirofuchi's doctoral thesis:
    https://www.usenix.org/legacy/events/usenix05/tech/freenix/hirofuchi/hirofuchi.pdf  
    `[saved:usenix.org__usenix05__hirofuchi.pdf]`

- Hirofuchi's conference paper:
    https://library.naist.jp/mylimedio/dllimedio/showpdf2.cgi/DLPDFR005317_P1-119  
    ~~`[saved:library.naist.jp__DLPDFR005317_P1-119.pdf]`~~

---


## USB/IP on Android

- "USB/IP Server" is an Android app
    * Open-source (GPL v3.0), beta 0.2, last commit 2014, last checked 2021-04
    * [GitHub repo][usbipserver-github] `[saved:@cgutman__USBIPServerForAndroid]`
    * [Play Store page][usbipserver-play]

[usbipserver-github]: https://github.com/cgutman/USBIPServerForAndroid
[usbipserver-play]: https://play.google.com/store/apps/details?id=org.cgutman.usbipserverforandroid


### Personally Tested

I have personally tested it with:

- Operating systems:
  * Server: Android Go 8.1
  * Client: Ubuntu 18.04 LTS x64

- Devices:
    * Mouse (details ???)
    * Keyboard (details ???)
    * Gamepad (details ???)
    * Flash drive (details ???)


In order to have an easy and a stable access to the "server" (my phone),
I've added a DHCP reservation (in my router) for my Android device:
`<Android phone MAC address> | 192.168.1.42`

Why 42?
  * Anrakki is 42
  * Anhappi is 4
  * PS: Both 4 and 42 are unhappy numbers (in math), as well as unlucky in Japanese culture.
TODO: Move to `hostnicknames.md`:

---


## USB/IP on Windows

- https://github.com/cezanne/usbip-win
    * "This project aims to support both a USB/IP server and a client on Windows platform."
    * KAITO: In C, 767 stars, last commit 2021-03, last checked 2021-04.

### Tested

- [ ] TODO: Actually make it work!

- [x] install Dev-C++: https://sourceforge.net/projects/orwelldevcpp/
    * [ ] Maybe just use VS...

- https://github.com/libusb/libusb/wiki/Windows
    * [x] Zadig: [Zadig - USB driver installation made easy](https://zadig.akeo.ie)

---

END.
