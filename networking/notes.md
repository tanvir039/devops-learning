# Networking Notes

These notes cover the fundamentals of computer networking as part of my DevOps learning journey.

---

## What is Networking?
Networking is the practice of connecting computers and devices so they can communicate and share resources.

---

## Key Networking Concepts

### IP Addresses
An IP address is a unique identifier assigned to a device on a network.

- IPv4 example: `192.168.1.1`
- IPv6 example: `2001:0db8:85a3::8a2e:0370:7334`

### MAC Addresses
A MAC address is a hardware address assigned to a network interface.

- Example: `00:1A:2B:3C:4D:5E`

---

## Ports and Protocols

### Common Protocols
- **HTTP (80)** – Web traffic
- **HTTPS (443)** – Secure web traffic
- **SSH (22)** – Remote access
- **FTP (21)** – File transfer

### TCP vs UDP
| TCP | UDP |
|-----|-----|
| Reliable | Fast |
| Ordered | Unordered |
| Used by HTTP, SSH | Used by DNS, streaming |

---

## Networking Commands (Linux)

### Check IP address
```bash
ip a
ping google.com
