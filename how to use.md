

`HTTP`: This attack consists of exhausting the victim by sending a huge amount of HTTP GET requests, eventually taking it down and preventing others to access its resources.

```
├─── DOS TOOL
├─── AVAILABLE METHODS
├─── LAYER 7: HTTP | HTTP-PROXY | SLOWLORIS | SLOWLORIS-PROXY
├───┐
│   ├───METHOD: HTTP
│   ├───TIME: 600
│   ├───THREADS: 800
│   └───URL: https://
```

`Slowloris`: Just like an HTTP attack, Slowloris also aims to block other users from accessing a certain resource, but it does that by connecting virtual hosts with a slow connection to the victim. The victim will eventually have a lot of slow connections open and will block new users from accessing its resources.

```
...
├───┐
│   ├───METHOD: SLOWLORIS
│   ├───TIME: 300
│   ├───THREADS: 200
│   ├───SLEEP TIME: 15
│   └───URL: https://
```

Both `HTTP` and `Slowloris` attacks have a proxy version. If you choose to use proxy, then the threads will initialize and connect to elite-anonymity public proxies, and if not, your IP will be used on the requests. We do not own the proxy servers and do not respond for anything that they may do (like leaking your actual IP); they are hosted by volunteers and their addresses are retrieved through the [Proxy Scrape API](https://docs.proxyscrape.com/).

<br>

## POSIX attacks only

To perform the following attacks you'll need a machine running a POSIX system, like Ubuntu. 
<br><br>

`SYN-Flood`: This attack relies on how the Tansmission Control Protocol (TCP) connections are designed. It takes advantage of the TCP 3-Way Handshake (SYN, SYN-ACK and ACK) by sending a lot of packets with the SYN flag, but never responding to the SYN-ACK packets sent by the victim, which makes it to wait forever with an open connection. If the victim somehow does not close the connection opened by the SYN packets, then it'll eventually block new connections.

```
...
├─── LAYER 4: SYN-FLOOD
├───┐
│   ├───METHOD: SYN-FLOOD
│   ├───TIME: 40
│   ├───THREADS: 10
│   └───URL: 192.168.0.1
```

`ARP-Spoof`: This attack works on layer 2 of the OSI model, specifically on the Address Resolution Protocol (ARP). It consists of sending an adulterated packet to the victim saying that we are the gateway of the local network, so the victim must send all its packets to our machine. We also tell the gateway that we are the victim; that way we become the man in the middle of the connection and can inspect all of the victims' packets with an analyzer.

```
...
├─── LAYER 2: ARP-SPOOF | DISCONNECT
├───┐
│   ├─── METHOD: ARP-SPOOF
│   │
│   ├─── [!] Scanning Local Network...
│   │
│   ├─── Avaliable Hosts:
│   │
│   │     192.168.0.102
│   │     192.168.0.105
│   │
│   ├─── IP: 192.168.0.102
│   ├─── TIME: 100
```

`Disconnect`: It blocks the victim from accessing the internet on the local network during the time the attack is happening.

```
...
├─── LAYER 2: ARP-SPOOF | DISCONNECT
├───┐
│   ├─── METHOD: DISCONNECT
│   │
│   ├─── [!] Scanning Local Network...
│   │
│   ├─── Avaliable Hosts:
│   │
│   │     192.168.0.100
│   │     192.168.0.103
│   │     192.168.0.105
│   │
│   ├─── IP: 192.168.0.100
│   ├─── TIME: 600
```

---
This application is intended to be used as a testing tool against your own servers. **DO NOT USE IT TO ATTACK OTHER PEOPLE**, we don't take responsibility for anything that may come up if you attack someone else. Also, this project makes a `DoS` attack, if you want to take down well-hosted servers, then it's up to you to scale the attack using a `DDoS` approach. Know the limitations of what you can do, and the defense mechanism used by your target; for instance, if a webserver uses DDoS mitigation appliances (such as load balancing), then you'll probably fail to take it down; a router that implements SYN Cookies will not be affected by a SYN-Flood attack, and so on.