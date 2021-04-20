---
date: 2021-04-11
2021-04-11_usb-condom.md
---

# About USB Attacks
#usb

## android_hid

[@androidmalware/android_hid][].
    * 2021-04-11 [@androidmalware/android_hid][] was github trending
    * Demos:
        + [Hacking into Android in 32 seconds | HID attack | Metasploit | PIN brute force PoC - YouTube](https://www.youtube.com/watch?v=aOWr6rWhsIs)
        + [Getting remote access to PC with Android | HID attack | Metasploit | Android Termux - YouTube](https://www.youtube.com/watch?v=PJbqZm73MOc)

[@androidmalware/android_hid]: https://github.com/androidmalware/android_hid
[@pelya/android-keyboard-gadget]: https://github.com/pelya/android-keyboard-gadget

> ### How to prevent this happening on PC
> 1. Don't let anyone charge their smartphones in your PC
> 2. Use security software that will detect Metasploit payload
> 3. USB condom should help


[@androidmalware/android_hid][]
uses [@pelya/android-keyboard-gadget][]'s `hid-gadget-test` binary.

[@androidmalware/android_hid][]
references [@anbud/DroidDucky](https://github.com/anbud/DroidDucky).
> DroidDucky is a duckyscript interpreter written in Bash which brings all of ducky scripting goodness to Android.

---

- https://github.com/hak5darren/USB-Rubber-Ducky `[interesting]`
    
    * https://github.com/hak5darren/USB-Rubber-Ducky/wiki/Duckyscript
    
    * > The USB Rubber Ducky is a Human Interface Device programmable with a simple scripting language allowing penetration testers to quickly and easily craft and deploy security auditing payloads that mimic human keyboard input. The source is written in C and requires the AVR Studio 5 IDE from atmel.com/avrstudio. Hardware is commercially available at hakshop.com. Tools and payloads can be found at usbrubberducky.com. Quack!

---

## ConfigFS

[@pelya/android-keyboard-gadget] references [@tejado/android-usb-gadget](https://github.com/tejado/android-usb-gadget):
> For newer Kernel versions (>= 3.19) the patch is not anymore required and can be replaced by ConfigFS (USB Gadget Tool).


## USB condom
Basically use a USB male-to-female connector/adapter(?) to turn a USB data cable to a USB charge-only cable.


**Alternatives**:
> Android devices commonly prompt the user before allowing the device to be mounted as a hard drive when plugged in over USB.
> Since release 4.2.2, Android has implemented a whitelist verification step to prevent attackers from accessing the Android Debug Bridge without authorization.
>
> -- https://en.wikipedia.org/wiki/Juice_jacking


**Interesting reads**:
- [Making a USB Condom by MaxusMetalworks | Instructables](https://www.instructables.com/Making-a-USB-Condom/)
    * KAITO: DIY, just for the sake of curiosity. **Check the comments!**
- [Does Your Phone Charger Need A ‘USB Condom’ When You Travel? by Suzanne Rowan Kelleher](https://www.forbes.com/sites/suzannerowankelleher/2020/02/24/does-your-phone-charger-need-a-usb-condom-when-you-travel/?sh=2f9513ff33b7)

---

END.
