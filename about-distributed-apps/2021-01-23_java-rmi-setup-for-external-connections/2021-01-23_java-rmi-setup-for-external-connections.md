---
date: "2021-01-23"
permalink: "/java-rmi-setup-for-external-connections/"
...

# Java RMI: External Connections

## Terminology (meaningless junk)

<details>

- <i lang="fr">binôme</i> means teammate, as in "<i lang="fr">programmation en binôme</i>" ("pair programming").

- "**Kurage**" alliterates with "Kaito". Also, "kurage" means "jellyfish" in Japanese.
(Get it? Kurage-Kaito? Jellyfish-Djalil? Nobody cares.)

- **San Kyu** literally means "3 9" in Japanese, and it is used colloquially to mean "thank you".

- **Shi Ni** literally means "4 2" in Japanese. "Shini" can mean "death"/"to death".

... This section should (#todo) be moved to `my-hostnicknames.md`.

</details>


## Problem: Can't connect to the Java RMI "server" at all

### TLDR

**Facts**:
- We, peasants, connect to the internet using a "modem router" that is assigned a dynamic IP address.
- Routers use NAT (think: random ports) to represent all devices in the local network with one external IP.
- You can assign a fixed IP address for your local device either by statically setting it in the host machine or by adding a DHCP Reservation entry in your router (if supported).
- Practically most modern routers support Port Forwarding (and maybe Port Triggering).
With Port Forwarding, you forward any packet sent to your theExternalIP:PortX to a specific host in the local network, say, someInternalIP:PortY. (PortX and PortY may or may not be the same).
- Java RMI's Registry has a default port (whatever it is), but you can change it to whatever you want.
- Java RMI's exported objects use random ports if you don't specify a port (or if you pass zero). In other words, you can specify ports.
- Java RMI's ports of concern to us are those of "servers" (that are providing remote objects or the registry itself). Clients only need to access them. 

**The main problem**:
- Clients requesting the registry or the exported Remote objects cannot pass through the router and reach the server.

**The (incomplete) solution**:
- Java: Explicitly specify ports for the RMI Registry and exported objects (when calling `super(somePort)` in the Remote object's constructor or when calling `UnicastRemoteObject.exportObject(obj, somePort)`).
- Java/However: Find your external IP address and use it in the Java code instead of localhost or your LAN address.
- Router or Server: Set a static IP address for the server (say, the **Unlucky** server is at 192.168.1.13).
- Router: Set Port Forwarding to forward packets to their correct server's ports. Say, :portA to Unlucky:portA for RMI Registry, and :portB to Unlucky:portB for the StockImpl obj.
- Java: Now, if all clients (1, 2, 3) are external, they will work with the server.

That's the general idea.  
It is totally *meh* and, worse, some problems still exist.
We can do better...


## Problem: That's too much, man. Do I have to do it manually for each port?

Maybe not. This is what I have done (&sect;_Using port forwarding and port triggering_)...

***Or, for a simpler solution, just [use a port mapping app (see upnp-port-forwarding.md)](../2021-01-22_upnp-port-forwarding/).***

### Choosing a port range

"Is there any cool number we can use for a #SmartHospital-related thing?" I asked myself.

- 666000 sounded cool ("all hell's gates/ports are open"), but it's invalid
(65535 is the **MAX PORT NUMBER**).

- 42000 ("Shi Ni" / "To Death") is what I settled with.

The problem was with ports...  
Remember that:
- Remote objects are shared on random ports if you don't specify them (or if you pass `0`).
- Remote object names must match when calling both `bind`/`rebind` ("exporting") and `lookup` ("importing").

I thought to use some `<Whatever>Things` enum as a way to document object names and their corresponding, fixed ports.
This enum solves two issues at once:
1. Fixing ports.
2. Ensuring that object "names" are correct: to avoid stupid mistakes like passing "ConTRolEuR" instead of "Controleur" or "CONTROLEUR".

So, Port 42000 will be for the RMI Registry.  
As for remote objects, they will be exposed on ports 42001-42999 (just "my semantics," there's no technical reason).  
The only object we are exporting in the second Java RMI assignment is an instance of `ControleurImpl`.
I've chosen to allocate the first spot to it: "CONTROLEUR" -> Port 42001.

To document object name-port pairs, I created a new `enum`, `SmartHospitalThing`:
```java
/**
 * Remote things. Their names and ports.
 */
public enum SmartHospitalThing {

    /** RMI Registry */
    RMI_REGISTRY (42000),

    /** ControleurImpl object */
    CONTROLEUR(42001);
    
    // ...
}
```

### Checking used ports
Let's check the open ports: `sudo netstat -ntlp | grep LISTEN`
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
[...]
tcp6       0      0 :::139                  :::*                    LISTEN      13030/smbd          
tcp6       0      0 :::42000                :::*                    LISTEN      24097/java          
tcp6       0      0 :::42001                :::*                    LISTEN      24097/java          
[...]
```

### Using port forwarding and port triggering

Using what I've already learned from Oracle docs, Wikipedia, and about my modem's supported "features," I was able to solve it using **port forwarding** and **port triggering**.

My router didn't let me specify a port range when forwarding. I absolutely did not want to specify each port manually. (Imagine if we had 
<i lang="fr">un nombre important</i> of remote objects. Totally unacceptable!)  
So, eh, I've used a ~~disadvantage~~ "feature" of port triggering (["that it binds the triggered port to a single client at a time" - Wikipedia][wiki:Port triggering]), to forward all ports to the same machine.


**Port forwarding** config:  
Like, forward packets sent to TCP Port 42000 _specifically_ to Sankyu/192.168.1.39.  
![Port Forwarding page on my D-Link router](./screenshots/dlink-router--port-forwarding.png)


**Port triggering** config:  
Like, when a request to the RMI Registry's port (42000) is made, forward all related ports (42001-42999) to _whatever_ machine that triggered this rule (which is Sankyu).  
![Port Triggering page on my D-Link router](./screenshots/dlink-router--port-triggering.png)
> Port Trigger Table:
> ```
> | ServerName | Trigger Protocol | Direction | Match Port  | Open Protocol | Relate Port |
> |------------+------------------+-----------+-------------+---------------+-------------|
> |  SHINI     | both             | incoming  | 42000-42000 | both          | 42001-42999 |
> ```

---

## Problem: How to handle the dynamic nature of our external IPs?

You can use a **dynamic DNS** service like DuckDNS, No-IP, or an open-source alternative like `nsupdate.info` or whatever.  
I've tried only No-IP and DuckDNS and found the latter to be the simplest, most no-nonsense option.
I've been using DuckDNS for over two years now.

KAITO: I'm using `sankyu.djalil.me` like a personalized(?) alias for my DuckDNS domain (`kurage.duckdns.org`).  
`kurage.duckdns.org`'s A record is periodically updated by our DuckDNS client, ~~`KurageBot.js`~~ `DuckUpdater.java`.


## Problem: Can't access local machines using their external IP (NAT loopback issue)
Due to the NAT loopback problem, we can not access LAN "servers" using their external IPs or DDNS domains.

You may want to read what [ConnectWise writes about this issue][connectwise-nat].

Since I have only two machines, I opted for the simplest solution/workaround and edit the `hosts` file.
- Linux (Ubuntu 18.04): Edit `/etc/hosts`
- Windows (Windows XP): Edit `C:\WINDOWS\system32\drivers\etc\hosts`
And add an entry that resolves with a local address for the server:
```
# Thank You server
192.168.1.39 sankyu.djalil.me
```

![The modified hosts file is opened in Notepad, Windows XP.](./screenshots/winxp-hosts-file.png)

---

### Output of running `dig`

If resolved normally (the local `hosts` file is used):
```console
djalil@Sankyu:~$ dig sankyu.djalil.me

; <<>> DiG 9.11.3-1ubuntu1.13-Ubuntu <<>> sankyu.djalil.me
[...]

;; ANSWER SECTION:
sankyu.djalil.me.       0       IN      A       192.168.1.39

;; Query time: 0 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
[...]
```

---

If resolved explicitly using Google's public DNS server (i.e. the `hosts` file is not used):
```console
djalil@Sankyu:~$ dig @8.8.8.8 sankyu.djalil.me

; <<>> DiG 9.11.3-1ubuntu1.13-Ubuntu <<>> @8.8.8.8 sankyu.djalil.me
[...]

;; ANSWER SECTION:
sankyu.djalil.me.       299     IN      CNAME   kurage.duckdns.org.
kurage.duckdns.org.     59      IN      A       105.107.130.104

;; Query time: 541 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
[...]
```

## ~~Problem: Java RMI complains that hostname doesn't match~~

~~If both Alice (`Arisu.duckdns.org`) and Bat (`Batto.duckdns.org`) point to the same host (e.g. `Sankyu`),
but the server declares Alice and the client requests it as Bat, an `Exception` gets thrown.~~  
~~What's the use of hostname in Java RMI and why does it need to match?~~

- [ ] LEARN: What's the use of the hostname property anyway?  
I get the [`Host` header in HTTP (MDN)][mdn-http-host], but what about Java RMI similar?

---

## References

- [IPalyzer](https://ipalyzer.com)

- [What port is used by Java RMI connection? | Stack Overflow](https://stackoverflow.com/questions/3071376/what-port-is-used-by-java-rmi-connection)

- [Cannot access external IP address from LAN | ConnectWise][connectwise-nat] `[archived:20210119134637]`

- [A Proper Server Naming Scheme SSD VPS Cloud Hosting | mnx.io](https://mnx.io/blog/a-proper-server-naming-scheme/) `[interesting][archived:20210114215447]`

- [Standardizing Host and Server Naming Conventions | The Official Device42 Blog](https://www.device42.com/blog/2014/02/standardizing-host-and-server-naming-conventions/) `[interesting][archived:20210119113931]`

- Also see:
    * [wiki:Port triggering][]
    * [wiki:Port forwarding][]

[connectwise-nat]: https://docs.connectwise.com/ConnectWise_Control_Documentation/On-premises/On-premises_knowledge_base/Cannot_access_external_IP_address_from_LAN
[mdn-http-host]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Host
[wiki:Port triggering]: https://en.wikipedia.org/wiki/Port_triggering#Disadvantages
[wiki:Port forwarding]: https://en.wikipedia.org/wiki/Port_forwarding

---

END.
