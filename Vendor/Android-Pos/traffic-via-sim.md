Most POS developers assume that because the backend is only reachable over the device's **cellular data (3G/4G)**, nobody can intercept the traffic. The logic goes: *"If a proxy isn't on the same cellular network as the POS, it can't reach the server  so we're safe."*
 
This assumption is **wrong**, and the bypass is surprisingly simple.
 
---
 
## The Trick
 
A POS terminal runs on a **SIM card configured with a custom APN** (Access Point Name). The APN settings  including the access point address, port, and sometimes credentials  are visible on the POS itself. No authentication is needed to read them off the device.
 
**If you clone those APN settings onto any other Android phone, that phone is now on the same network segment as the POS.**
 
From there, create a mobile hotspot on that phone. Connect both the POS and your laptop (running Burp) to that hotspot. Configure the POS proxy settings to point to your laptop.
 
Now the traffic flows:
 
```
POS App → Burp (laptop) → Mobile Hotspot → Cellular APN → Backend Server
                                ↑
                    Same network as POS  server is reachable
```
 
The proxy can see the server. The server can see the proxy. Full interception achieved.
 
---
 
## Why This Works
 
The backend server is firewalled to accept connections **only from the carrier's APN network** (a private APN, not the public internet). Normally, a proxy on Wi-Fi or a different SIM can't reach it.
 
By cloning the APN config onto a phone with the same SIM (or same carrier profile), you gain the same network access the POS has  and can therefore route Burp's traffic through to the server transparently.
 
> 💡 **Key insight:** The SIM card is the network credential. The APN config is publicly readable on the device. Physical access to the POS = network access.
 
---
 
## Steps to Reproduce
 
### Prerequisites
- Target POS terminal (physical access required)
- Android phone with a SIM slot
- Laptop with Burp Suite
- The POS SIM card (or a SIM on the same carrier with the same APN access)
### Step 1  Extract APN Settings from the POS
 
Navigate to the APN settings on the POS. Note down:
- APN name
- APN address / host
- Port (if configured)
- Username / Password (if any)
- MCC / MNC
These are visible in plain text in the device's network configuration menu.
 
### Step 2  Clone the APN on Your Android Phone
 
On your Android phone:
 
```
Settings → Mobile Networks → APN → Add New APN
```
 <img width="531" height="898" alt="image" src="https://github.com/user-attachments/assets/020606a3-ac48-4853-aab1-971d82f3de55" />

Enter the exact values copied from the POS. Save and select this APN as the active one.
 
Now your phone has the same network access as the POS.
 
### Step 3  Create a Hotspot
 
Enable the mobile hotspot on your Android phone. This bridges the cellular APN to a Wi-Fi network your laptop can join.
 
### Step 4  Connect POS and Laptop to the Hotspot
 
- Connect your laptop to the phone's hotspot.
- Connect the POS to the same hotspot (via its Wi-Fi settings).
- On your laptop, note the IP address assigned by the hotspot (e.g., `192.168.x.x`).
### Step 5  Configure Burp Suite
 
On your laptop:
- Open Burp Suite.
- Set the proxy listener to `0.0.0.0:8080` (so it listens on all interfaces, including the hotspot IP).
- Install the Burp CA certificate on the POS if needed for HTTPS.
### Step 6  Configure the POS Proxy
 
On the POS, navigate to proxy / network settings and set:
 
```
Proxy Host: <laptop's hotspot IP>
Proxy Port: 8080
```
 
### Step 7  Intercept
 
Open the payment application on the POS and perform any action (settlement, configuration load, payment). Observe the full HTTP/HTTPS traffic in Burp.
 
---
