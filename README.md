# GFW-2021Summer-TLS-Proxy-Attack
Thoughts, discussions, and clarifications on the attack against TLS Proxies initiated by GFW in 2021 Summer (June-July)

## Background

Starting in 2021 June, the Great Firewall, operated by the Chinese government, started blocking and interfering with the widely used TLS proxies in China. Despite those which have got their port 443 blocked from Chinese users, some target proxy servers started to experience SSL/TLS handshake failure caused by RST injection. Meanwhile, the HTTPS website is **still accessible**.

## Initial guesses

- GFW can analyze the request sent from TLS proxy clients to the server.
  - False. The RST happens during the TLS handshake where no actual proxy-like behavior has started.
- GFW checks against TLS Fingerprints exposed in ClientHello and selectively attacks against infamous TLS fingerprints or allow only clients with famous TLS fingerprints to pass.
  - False. Forging the TLS fingerprint from a well-known browser does not improve the connectivity.

## Data (n=50)

<img src="https://raw.githubusercontent.com/Gaukas/GFW-2021Summer-TLS-Proxy-Attack/master/data/Stat.png">

- Note: Node name may not reflect the actual routing and/or server physical location. 
- **Latency (ms):** 65, 69, 93, 191, 63, 199, 131, 133
- **Packet loss:** 8%, 12%, 2%, 0%, 0%, 0%, 0%, 0%

<img src="https://raw.githubusercontent.com/Gaukas/GFW-2021Summer-TLS-Proxy-Attack/master/data/SNI_verification.png">

## Rough conclusion

- GFW **does not show the ability to identify TLS proxy traffics** from normal HTTPS web browsing traffics **in real-time**.
- GFW **isn't utilizing a purely fingerprint-based discrimination**. We don't know if fingerprint matters in current state but it is not a major decision factor.
- Statistics gives **a strong signal about a backend IP reputation system**. This may indicate that GFW is analyzing the TLS traffic and there exists difference between proxy traffic and real web browsing traffic.
- GFW **isn't incorporating SNI sniffing check** in this attack.

## Credits 

- [@ewust](https://github.com/ewust), [@jhalderm](https://github.com/jhalderm), [@jmwample](https://github.com/jmwample)
- [Refraction Networking/uTLS](https://github.com/refraction-networking/utls)
- [TLSfingerprint.io](https://tlsfingerprint.io/)
