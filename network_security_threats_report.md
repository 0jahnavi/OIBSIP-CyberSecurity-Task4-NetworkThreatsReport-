# Task 4 – Research Report: Common Network Security Threats

## Objective

This report examines common network security threats, explains how each attack works, discusses a real-world incident and its impact, and presents specific mitigation strategies that network administrators can apply.

---

## 1. Introduction

Network security is essential because modern organizations depend on networks to provide access to websites, cloud services, applications, databases, communication systems, and business operations. A network attack can affect the **confidentiality, integrity, or availability** of these services. Common threats such as DoS/DDoS attacks, Man-in-the-Middle (MITM) attacks, IP spoofing, and DNS poisoning/spoofing exploit weaknesses in network protocols, trust relationships, or infrastructure. Their consequences can range from temporary service outages to credential theft, financial loss, data interception, and redirection of users to malicious websites. Therefore, network administrators must understand how these attacks operate and apply layered controls such as traffic filtering, encryption, authentication, network segmentation, DNS security, monitoring, and incident-response procedures.

---

# 2. DoS/DDoS Attacks

## 2.1 What is DoS/DDoS?

A **Denial-of-Service (DoS)** attack attempts to make a network, server, application, or service unavailable to legitimate users by exhausting resources.

A **Distributed Denial-of-Service (DDoS)** attack performs the same basic objective using many systems or sources. These systems may be compromised devices in a botnet, rented infrastructure, or third-party servers abused for reflection/amplification.

MITRE ATT&CK classifies Network Denial of Service as **T1498**, with two major sub-techniques:

- **T1498.001 – Direct Network Flood**
- **T1498.002 – Reflection Amplification**

A network DoS attack can exhaust bandwidth or overload network devices and services. DDoS attacks are especially difficult to handle because legitimate users and malicious traffic may arrive simultaneously.

## 2.2 How a DoS/DDoS Attack Works

A typical DDoS attack can work as follows:

1. The attacker identifies a target such as a website, API, DNS service, or network gateway.
2. The attacker obtains control over or access to many traffic-generating systems.
3. These systems send a large volume of packets or requests toward the target.
4. The traffic consumes bandwidth, connection capacity, CPU, memory, or application resources.
5. Legitimate users experience slow response times, errors, or complete service unavailability.
6. The organization applies filtering, traffic diversion, rate limiting, or other mitigation to restore availability.

### Reflection and amplification

In a reflection attack, the attacker sends requests to third-party servers while using the victim's IP address as the forged source address. The third-party servers then send their responses to the victim.

If the response is much larger than the original request, the attack becomes an **amplification attack**. DNS, NTP, and other UDP-based services have historically been abused for this purpose.

## 2.3 Real-World Example – GitHub DDoS Attack (2018)

On **February 28, 2018**, GitHub experienced a major DDoS attack. GitHub reported that the service was unavailable from **17:21 to 17:26 UTC** and intermittently unavailable until approximately 17:30 UTC.

The attack used a **memcached-based amplification technique** and reached a peak of approximately **1.35 Tbps and 126.9 million packets per second**. GitHub redirected traffic to Akamai, where the attack traffic was filtered. GitHub reported full recovery at approximately 17:30 UTC.

The incident demonstrates how an amplification-based DDoS can generate extremely large traffic volumes without requiring the attacker to send an equivalent amount of traffic directly from their own infrastructure.

## 2.4 Impact

DDoS attacks can cause:

- Website and application downtime.
- Loss of availability for legitimate customers.
- Financial losses caused by interrupted business operations.
- Increased bandwidth and mitigation costs.
- Damage to an organization's reputation.
- Increased workload for network and security teams.
- Disruption of dependent services such as APIs, DNS, email, or authentication.
- Potential distraction from another attack occurring at the same time.

DDoS attacks primarily affect the **availability** part of the CIA triad.

## 2.5 Three Specific Mitigation Strategies

### 1. Use upstream DDoS protection/CDN services

Route public-facing services through a DDoS mitigation provider or CDN capable of absorbing and filtering large traffic volumes before they reach the organization's network.

### 2. Apply traffic filtering and rate limiting

Use firewalls, routers, load balancers, WAFs, and upstream filtering to block abnormal traffic and limit excessive requests. Filtering can be based on source, destination, protocol, port, packet characteristics, or application behavior.

### 3. Prepare redundancy and an incident-response plan

Use redundant links, geographically distributed infrastructure, scalable hosting, continuous traffic monitoring, and a documented DDoS response plan. The organization should also maintain emergency contacts for its ISP and DDoS mitigation provider.

---

# 3. Man-in-the-Middle (MITM) Attacks

## 3.1 What is a MITM Attack?

A **Man-in-the-Middle (MITM)** attack, also called **Adversary-in-the-Middle (AiTM)** in MITRE ATT&CK terminology, occurs when an attacker positions themselves between two communicating parties.

Instead of:

`Client <----> Legitimate Server`

the communication becomes:

`Client <----> Attacker <----> Legitimate Server`

The attacker may observe, capture, relay, or modify traffic.

MITRE ATT&CK identifies **T1557 – Adversary-in-the-Middle**. Examples of ways attackers can establish this position include:

- ARP cache poisoning.
- DNS/name-resolution poisoning.
- DHCP spoofing.
- Malicious Wi-Fi/Evil Twin networks.
- Malicious proxy infrastructure.
- Weak or improperly validated TLS/certificates.

## 3.2 How a MITM Attack Works

A general MITM attack can occur in these stages:

1. The attacker identifies a communication path between a victim and a legitimate service.
2. The attacker manipulates routing, name resolution, wireless access, ARP, DHCP, or another mechanism.
3. The victim's traffic is redirected through infrastructure controlled by the attacker.
4. The attacker observes or relays the communication.
5. If encryption/authentication is weak or incorrectly validated, the attacker may capture credentials or modify information.
6. The attacker attempts to remain unnoticed while the victim believes the connection is legitimate.

Modern HTTPS/TLS significantly reduces the effectiveness of many traditional MITM attacks when certificates are correctly validated and secure cryptographic configurations are used.

## 3.3 Real-World Example – DigiNotar (2011)

The **DigiNotar** incident is a well-known example involving fraudulent digital certificates.

In 2011, attackers compromised DigiNotar, a certificate authority. Fraudulent certificates were created for hundreds of websites, including major services such as Google and Skype. Reports indicated that the fraudulent certificates were used to intercept or monitor users, particularly in Iran.

The incident demonstrated why certificate authorities are a critical part of Internet trust. If an attacker obtains a certificate trusted by browsers, they may be able to impersonate a legitimate website and establish an interception position.

## 3.4 Impact

MITM attacks can result in:

- Theft of usernames and passwords.
- Theft of session cookies or authentication tokens.
- Exposure of confidential communications.
- Modification of messages or transactions.
- Phishing and website impersonation.
- Unauthorized access to accounts.
- Financial fraud.
- Loss of confidentiality and integrity.

## 3.5 Three Specific Mitigation Strategies

### 1. Enforce TLS/HTTPS and validate certificates

Use modern TLS for sensitive communications and ensure that clients correctly validate certificates. Avoid accepting certificate warnings without investigation.

### 2. Secure local networks and wireless access

Use WPA2/WPA3 Enterprise where appropriate, strong authentication, secure switch configurations, and network segmentation. Disable unnecessary legacy protocols such as LLMNR where possible because they can be abused for name-resolution poisoning.

### 3. Use strong authentication and monitoring

Use phishing-resistant MFA where possible, monitor for unusual authentication activity, and use network intrusion detection to identify abnormal ARP/DNS/TLS behavior. MFA reduces the damage from stolen passwords, although it should not be treated as a replacement for encrypted communication.

---

# 4. IP Spoofing

## 4.1 What is IP Spoofing?

**IP spoofing** occurs when an attacker sends a packet with a forged source IP address.

Normally, a packet contains the address of the system that sent it. With spoofing, the attacker changes the source address so that the packet appears to originate from another system.

IP spoofing can be used for:

- Hiding or making the real source harder to trace.
- Bypassing poorly designed source-IP-based access controls.
- Reflection attacks.
- DDoS attacks.
- Sending malicious traffic that appears to come from a trusted address.

IP spoofing is usually a **technique or component of an attack**, rather than a complete attack by itself.

## 4.2 How IP Spoofing Works

A simplified process is:

1. The attacker creates network packets.
2. The attacker places a forged source IP address in the packet.
3. The packet is transmitted toward the target or an intermediary service.
4. The receiving system sees the forged address as the apparent source.
5. Depending on the protocol and attack type, the system may respond to the spoofed address or trust the packet.
6. The attacker can use this behavior for reflection, access-control abuse, or to make attribution more difficult.

IP spoofing is especially useful with connectionless protocols such as UDP because they do not establish a normal connection before sending data.

## 4.3 Real-World Example – Memcached Amplification DDoS

The **2018 GitHub DDoS attack** is also a useful real-world example of IP spoofing because memcached amplification attacks rely on forged source IP addresses.

In a reflection/amplification attack, the attacker sends requests to publicly reachable UDP services while placing the victim's IP address in the source field. The intermediary service sends its larger response to the victim.

GitHub reported that its 2018 attack peaked at approximately 1.35 Tbps. The incident illustrates how source-address spoofing can enable reflection and amplification attacks.

## 4.4 Impact

IP spoofing can:

- Make attack traffic harder to trace.
- Enable reflection/amplification attacks.
- Circumvent weak IP-based authentication or access-control rules.
- Cause systems to send responses to an innocent third party.
- Increase the scale and effectiveness of DDoS attacks.
- Reduce the reliability of source-IP information in security logs.

## 4.5 Three Specific Mitigation Strategies

### 1. Implement BCP 38 ingress filtering

ISPs and network administrators should filter traffic entering a network when the source address is not valid for the network from which it arrived. This is recommended by **IETF BCP 38 / RFC 2827**.

### 2. Use source-address validation/uRPF

Routers can use **Unicast Reverse Path Forwarding (uRPF)** or other source-address validation mechanisms to reject packets whose source addresses are inconsistent with routing expectations.

### 3. Avoid trusting source IP addresses alone

Do not use an IP address as the only authentication mechanism for sensitive systems. Combine network filtering with strong identity-based authentication, encryption, logging, anomaly detection, and appropriate firewall rules.

---

# 5. DNS Poisoning/Spoofing

## 5.1 What is DNS?

The **Domain Name System (DNS)** translates human-readable domain names such as a website name into IP addresses.

DNS poisoning/spoofing occurs when an attacker causes a victim or DNS resolver to receive a false DNS answer.

For example:

`legitimate-bank.example → legitimate IP`

may be replaced with:

`legitimate-bank.example → attacker-controlled IP`

A victim can then be redirected to a malicious website even though they entered the correct domain name.

## 5.2 How DNS Poisoning/Spoofing Works

A simplified cache-poisoning attack works like this:

1. A user's device asks a recursive DNS resolver for the IP address of a domain.
2. The attacker attempts to inject a fraudulent DNS response.
3. If the resolver accepts the forged response, it stores the incorrect information in its cache.
4. Other users querying the same resolver may receive the malicious IP address.
5. Users are redirected to an attacker-controlled service.
6. The attacker may use the fake service for phishing, credential theft, malware delivery, or fraud.

DNS spoofing can also occur through compromised DNS accounts, DNS server compromise, router compromise, or BGP/routing manipulation.

## 5.3 Real-World Example – MyEtherWallet DNS/BGP Hijacking (2018)

On **April 24, 2018**, attackers used a **BGP route hijacking** technique involving Amazon Route 53 infrastructure to redirect some MyEtherWallet traffic to a malicious website.

Users attempting to access MyEtherWallet could be redirected to a fraudulent copy of the website. The incident resulted in cryptocurrency theft from users who entered sensitive information into the malicious site.

The incident is important because it demonstrates that attacks against DNS and Internet routing infrastructure can redirect users even when they enter the correct website address.

## 5.4 Impact

DNS poisoning/spoofing can cause:

- Redirection to fake websites.
- Credential theft.
- Phishing.
- Malware delivery.
- Financial theft.
- Loss of trust in online services.
- Disruption of access to legitimate services.
- Large-scale redirection if a shared DNS resolver or infrastructure component is affected.

DNS attacks can affect both **integrity** and **availability** of network services.

## 5.5 Three Specific Mitigation Strategies

### 1. Deploy DNSSEC

**DNS Security Extensions (DNSSEC)** digitally sign DNS data so that validating resolvers can verify its authenticity and detect tampering. DNSSEC is one of the strongest controls against forged DNS responses and cache poisoning.

### 2. Secure and harden DNS infrastructure

Keep DNS server software updated, restrict administrative access, disable unnecessary services, use secure configurations, and protect DNS management accounts with strong authentication and MFA.

### 3. Monitor DNS activity and use protective/validated resolvers

Monitor DNS logs for unexpected changes, unusual query patterns, and suspicious destinations. Use properly configured validating recursive resolvers and consider protective DNS services to block known malicious domains.

---

# 6. Comparison of Network Security Threats

| Threat | Main Attack Vector | Who/What Is at Risk? | Difficulty to Execute | Ease of Mitigation |
|---|---|---|---|---|
| DoS/DDoS | High-volume traffic, botnets, reflection/amplification | Websites, APIs, DNS, servers, network links, users | Medium to High | Medium |
| MITM/AiTM | ARP/DNS/DHCP manipulation, rogue Wi-Fi, proxying, certificate abuse | Users, credentials, sessions, sensitive communications | Medium to High | Medium to High |
| IP Spoofing | Forged source IP addresses | Networks, DDoS targets, third-party reflectors, weak IP-based controls | Low to Medium for basic spoofing; higher for effective attacks | Medium |
| DNS Poisoning/Spoofing | Forged DNS responses, compromised DNS infrastructure, routing/DNS manipulation | Users, websites, DNS resolvers, online services | Medium to High | Medium to High |

### Comparison Summary

**DoS/DDoS:** The primary objective is to reduce availability. Large attacks often require upstream filtering because the victim's own network connection may become saturated before traffic reaches its firewall.

**MITM:** The main danger is interception or manipulation of communications. Strong encryption, certificate validation, secure network configuration, and authentication significantly reduce the risk.

**IP Spoofing:** Spoofing is frequently a supporting technique used in larger attacks, particularly reflection/amplification DDoS. Network-level source validation is an important defense.

**DNS Poisoning/Spoofing:** The main danger is that users can be redirected to incorrect destinations. DNSSEC, secure DNS administration, validated resolvers, and monitoring help protect DNS integrity.

---

# 7. Conclusion – Three Key Takeaways for a Network Administrator

### 1. Use defense in depth

No single security control can stop every network attack. Combine firewalls, traffic filtering, encryption, authentication, network segmentation, DNS security, monitoring, and endpoint protection.

### 2. Protect the protocols and infrastructure that users trust

DNS, IP routing, TLS certificates, wireless networks, and network gateways are fundamental to normal communication. If attackers manipulate these trust mechanisms, they can affect many users at once.

### 3. Prepare before an attack happens

Network administrators should maintain monitoring and logging, regularly review exposed services, apply security updates, test incident-response procedures, and maintain communication channels with ISPs and security providers. Rapid detection and a prepared response can greatly reduce the impact of an attack.

---

# 8. References

1. MITRE ATT&CK – **Network Denial of Service (T1498)**  
   https://attack.mitre.org/techniques/T1498/

2. MITRE ATT&CK – **Adversary-in-the-Middle (T1557)**  
   https://attack.mitre.org/techniques/T1557/

3. CISA – **UDP-Based Amplification Attacks**  
   https://www.cisa.gov/ncas/alerts/ta14-017a

4. CISA/FBI/MS-ISAC – **Understanding and Responding to Distributed Denial-of-Service Attacks**  
   https://www.cisa.gov/sites/default/files/publications/understanding-and-responding-to-ddos-attacks_508c.pdf

5. GitHub – **February 28th DDoS Incident Report**  
   https://github.blog/news-insights/company-news/ddos-incident-report/

6. Microsoft – **Fraudulent Digital Certificates Could Allow Spoofing (DigiNotar)**  
   https://learn.microsoft.com/en-us/security-updates/securityadvisories/2011/2607712

7. RFC Editor – **RFC 2827 / BCP 38: Network Ingress Filtering**  
   https://www.rfc-editor.org/info/rfc2827

8. RFC Editor – **RFC 3704 / BCP 84: Ingress Filtering for Multihomed Networks**  
   https://www.rfc-editor.org/info/rfc3704

9. NIST – **SP 800-81 Rev. 3: Secure Domain Name System (DNS) Deployment Guide**  
   https://csrc.nist.gov/pubs/sp/800/81/r3/final

10. ICANN – **DNSSEC: What Is It and Why Is It Important?**  
    https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-05-en

11. MyEtherWallet – **Response to the DNS Hack of April 24, 2018**  
    https://medium.com/@myetherwallet/a-message-to-our-community-a-response-to-the-dns-hack-of-april-24th-2018-26cfe491d31c

---

## Submission Checklist

- [x] Report covers DoS/DDoS attacks.
- [x] DoS/DDoS includes working, real-world example, impact, and 3 mitigations.
- [x] Report covers MITM/AiTM attacks.
- [x] MITM includes working, real-world example, impact, and 3 mitigations.
- [x] Report covers IP spoofing.
- [x] IP spoofing includes working, real-world example, impact, and 3 mitigations.
- [x] Report includes the bonus DNS poisoning/spoofing section.
- [x] DNS poisoning/spoofing includes working, real-world example, impact, and 3 mitigations.
- [x] Comparison table includes attack vector, risk, difficulty, and mitigation.
- [x] Conclusion contains 3 key takeaways for a network administrator.
- [x] References section contains more than 4 credible sources.
- [x] No demo video is required for this research task.
