---
date: 2021-04-11
permalink: "uninstall-windows-product-key"
---

# Uninstall Windows Product Key
#Windows

Bring the watermark back!


Why
----

For reasons.


Steps
------

Run `cmd` as administrator (elevated).

Using `slmgr`: "Windows **S**oftware **L**icensing Management Tool" (**M**a**n**age**r**?).

**D**isplay **L**icense **V**erbose/detailed information:
```cmd
slmgr /dlv
```

**U**ninstall **P**roduct **K**ey:
```cmd
slmgr /upk
```

**C**lear **P**roduct **K**ey from the registr**y** ("prevents disclosure attacks"):
```cmd
slmgr /cpky
```


References
-----------

- [How to Uninstall Product Key to Deactivate Windows 10 | duvien.com](https://duvien.com/blog/how-uninstall-product-key-deactivate-windows-10)

---

END.
