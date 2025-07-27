# Networking Refresher

## Hardware Addressing

### 🔗 MAC Addresses (Media Access Control)

**Layer 2 - Data Link Layer**

MAC addresses serve as the fundamental hardware identification system for network devices.

```
MAC Address Structure: XX:XX:XX:XX:XX:XX (48 bits / 6 bytes)
                      └─────┘ └─────┘
                        OUI    Device
                   (First 3 bytes) (Last 3 bytes)
```

#### Key Characteristics:
- **Physical Address**: Permanently assigned to network interface hardware
- **Organizational Unique Identifier (OUI)**: First 3 bytes identify the manufacturer
- **Device Identifier**: Last 3 bytes provide unique device identification
- **Permanence**: Burned into hardware but can be spoofed via software

> **SOC Analyst Note**: MAC address spoofing is a common technique used by attackers to evade network access controls and blend in with legitimate traffic.

---

## Logical Addressing

### 🌐 IP Addresses (Internet Protocol)

**Layer 3 - Network Layer**

IP addresses provide logical addressing for network communication, operating above the physical hardware layer.

#### Version Comparison:

| Aspect | IPv4 | IPv6 |
|--------|------|------|
| **Address Length** | 32 bits (4 octets) | 128 bits (8 groups) |
| **Format** | 192.168.1.1 | 2001:0db8:85a3::8a2e:0370:7334 |
| **Address Space** | ~4.3 billion addresses | ~340 undecillion addresses |
| **Usage** | Primary internet protocol | Growing adoption |

#### IP Address Components:
```
IP Address = Network Portion + Host Portion
Example: 192.168.1.100/24
         └─────────┘ └─┘
         Network    Host
```

---

## Network Segmentation

### 📊 Subnetting Fundamentals

Subnetting divides larger networks into smaller, manageable segments using CIDR (Classless Inter-Domain Routing) notation.

**CIDR Notation Examples:**
- `/24` = 255.255.255.0 (254 usable hosts)
- `/16` = 255.255.0.0 (65,534 usable hosts)
- `/8` = 255.0.0.0 (16,777,214 usable hosts)

---

## Network Classifications

### 🏢 IP Address Classes and Private Networks

#### Traditional Class System:

```mermaid
graph TD
    A[IP Address Classes] --> B[Class A<br/>1-126<br/>Large Networks]
    A --> C[Class B<br/>128-191<br/>Medium Networks]
    A --> D[Class C<br/>192-223<br/>Small Networks]
    
    B --> B1[/8 Subnet<br/>~16M hosts]
    C --> C1[/16 Subnet<br/>~65K hosts]
    D --> D1[/24 Subnet<br/>254 hosts]
```

#### Private vs Public IP Ranges:

| Network Class | Private Range | CIDR | Typical Use |
|---------------|---------------|------|-------------|
| **Class A** | 10.0.0.0 - 10.255.255.255 | /8 | Large enterprises |
| **Class B** | 172.16.0.0 - 172.31.255.255 | /12 | Medium organizations |
| **Class C** | 192.168.0.0 - 192.168.255.255 | /16 | Home/small office |

> **Security Implication**: Private IP addresses are not routable on the internet, providing natural network segmentation and requiring NAT for external communication.

---

## Communication Protocols

### 🚪 Ports and Services

**Port Range Classification:**
- **0-1023**: Well-known/system ports (require admin privileges)
- **1024-49151**: Registered ports (applications and services)
- **49152-65535**: Dynamic/private ports (client connections)

#### Protocol Comparison:

```mermaid
graph LR
    A[Network Protocols] --> B[TCP<br/>Transmission Control Protocol]
    A --> C[UDP<br/>User Datagram Protocol]
    
    B --> B1[✓ Connection-oriented<br/>✓ 3-way handshake<br/>✓ Guaranteed delivery<br/>✓ Error checking]
    C --> C1[✓ Connectionless<br/>✓ Speed focused<br/>✓ No delivery guarantee<br/>✓ Minimal overhead]
    
    B --> B2[Use: File transfers, web browsing, email]
    C --> C2[Use: Streaming, DNS, gaming]
```

### 📋 Critical Ports for SOC Analysts

| Port | Protocol | Service | Security Notes |
|------|----------|---------|----------------|
| **20/21** | TCP | FTP | Unencrypted file transfer |
| **22** | TCP | SSH | Secure remote access |
| **23** | TCP | Telnet | ❌ Unencrypted remote access |
| **25** | TCP | SMTP | Email transmission |
| **53** | TCP/UDP | DNS | Name resolution |
| **80** | TCP | HTTP | ❌ Unencrypted web traffic |
| **88** | TCP/UDP | Kerberos | Authentication protocol |
| **110** | TCP | POP3 | Email retrieval |
| **123** | UDP | NTP | Time synchronization |
| **137** | UDP | NetBIOS | Windows networking |
| **143** | TCP | IMAP | Email access |
| **161** | UDP | SNMP | Network management |
| **389** | TCP | LDAP | Directory services |
| **443** | TCP | HTTPS | ✅ Encrypted web traffic |
| **445** | TCP | SMB | Windows file sharing |
| **500** | UDP | IKE | VPN key exchange |
| **514** | UDP | Syslog | Log transmission |
| **636** | TCP | LDAPS | ✅ Secure LDAP |
| **993** | TCP | IMAPS | ✅ Secure IMAP |
| **995** | TCP | POP3S | ✅ Secure POP3 |
| **3389** | TCP | RDP | Windows remote desktop |

> **SOC Alert**: Monitor unusual traffic on well-known ports and be especially vigilant for services running on non-standard ports.

---

## Network Protocol Stack

### 📚 OSI Model vs TCP/IP Implementation

Understanding how data flows through network layers is crucial for traffic analysis and incident investigation.

```mermaid
graph TD
    subgraph "OSI Model (7 Layers)"
        A7[7 - Application<br/>HTTP, FTP, SMTP]
        A6[6 - Presentation<br/>Encryption, Compression]
        A5[5 - Session<br/>Session Management]
        A4[4 - Transport<br/>TCP, UDP]
        A3[3 - Network<br/>IP, ICMP]
        A2[2 - Data Link<br/>Ethernet, WiFi]
        A1[1 - Physical<br/>Cables, Radio]
    end
    
    subgraph "TCP/IP Model (4 Layers)"
        B4[Application<br/>Layers 5-7]
        B3[Transport<br/>Layer 4]
        B2[Internet<br/>Layer 3]
        B1[Network Access<br/>Layers 1-2]
    end
    
    subgraph "Practical 5-Layer Model"
        C5[Application<br/>User Interface & Data]
        C4[Transport<br/>Segments & Services]
        C3[Network<br/>Packets & Routing]
        C2[Data Link<br/>Frames & MAC]
        C1[Physical<br/>Bits & Signals]
    end
```

#### Data Encapsulation Process:

| Layer | Data Unit | Header Added | Example |
|-------|-----------|--------------|---------|
| **Application** | Data | Application headers | HTTP request |
| **Transport** | Segments | TCP/UDP headers | Port numbers |
| **Network** | Packets | IP headers | Source/destination IPs |
| **Data Link** | Frames | Ethernet headers | MAC addresses |
| **Physical** | Bits | Physical encoding | Electrical signals |

---

## Network Security Architecture

### 🛡️ IDS/IPS Placement Strategy

**Optimal Positioning Principle**: Deploy intrusion detection and prevention systems where they share the same network segment as monitored endpoints.

```mermaid
graph TD
    A[Internet] --> B[Firewall]
    B --> C[IDS/IPS<br/>Same subnet as endpoints]
    C --> D[Internal Network<br/>192.168.1.0/24]
    D --> E[Endpoint 1<br/>192.168.1.10]
    D --> F[Endpoint 2<br/>192.168.1.11]
    D --> G[Endpoint 3<br/>192.168.1.12]
    
    style C fill:#ffeb3b
    style E fill:#e3f2fd
    style F fill:#e3f2fd
    style G fill:#e3f2fd
```

#### Benefits of Proper IDS/IPS Placement:
- **Visibility**: Monitor all traffic to/from critical assets
- **Context**: Same source IP visibility enables rapid asset identification
- **Response**: Faster incident response and threat containment
- **Coverage**: Comprehensive monitoring of lateral movement attempts

---

## SOC Analyst Applications

### 🔍 Network Traffic Analysis

Understanding these networking fundamentals enables SOC analysts to:

#### **Layer 2 Analysis**:
- Identify MAC address spoofing attempts
- Detect unauthorized devices on network segments
- Analyze ARP poisoning attacks

#### **Layer 3 Analysis**:
- Track IP address reputation and geolocation
- Identify unusual routing patterns
- Monitor for IP address conflicts and scanning

#### **Layer 4 Analysis**:
- Correlate service ports with expected applications
- Detect port scanning and service enumeration
- Identify covert channels and tunneling attempts

#### **Traffic Flow Monitoring**:
- Establish network baseline behavior
- Detect lateral movement patterns
- Identify data exfiltration attempts

---

## Summary

This networking refresher provides the foundational knowledge necessary for effective SOC operations:

- **Hardware Addressing**: MAC addresses for device identification
- **Logical Addressing**: IP addressing and subnetting concepts
- **Protocol Understanding**: TCP/UDP characteristics and common ports
- **Network Architecture**: Protocol stack layers and data flow
- **Security Positioning**: Strategic placement of monitoring tools

These concepts form the backbone of network traffic analysis, enabling SOC analysts to identify anomalies, investigate incidents, and respond to security threats effectively.

[⬆️ Back to Refreshers](./README.md)
