---
date: "2020-11-12"
permalink: "/windows-xp-restrict-network-access-to-lan/"
...

# Restrict Windows XP's network access to LAN
#WindowsXP #NetworkAdministration(?) #Sloth-related

This is not exactly what I was looking for, but I still wanted to share it:

So there's this question on Stack Exchange:

**Paraphrased**: "I'm running Windows XP (guest) in VirtualBox on Linux (host). I want to give the guest access to LAN devices (e.g. a network printer), but to no other network connection."

One of the answers suggests messing with the system's default gateway to limit access to only the local network (effectively preventing it from accessing other networks).

This suggestion made sense to me.

Even though it is not exactly the use-case I was looking for, it at least feels good to know that the stuff you've learned in school ("Network Administration" class) have real-life applications...

Screenshot: Windows XP running in VirtualBox on Linux with access to the internet.

Stack Exchange answer/question: https://superuser.com/a/1410004


![](./2020-11-12_windows-xp-ipconfig.png)
> ALT TEXT: \
> The output of executing the 'ipconfig' command in Windows XP's cmd. \
> It shows that the IP Address and Subnet Mask are set, while the Default Gateway is left empty. \
> In the taskbar, Internet Explorer's title reads "Cannot find server".

---

## View network config

**Start** > **Run...** > `cmd`
```cmd
ipconfig
```
This displays the system's network information, including the default gateway.

## Change network config

SEE: [[slow-server-virtualized]], "Restrict Access to the LAN" section.

---

END.
