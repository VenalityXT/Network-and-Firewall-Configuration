# 🧱 **Network and Firewall Configuration**

This project demonstrates how to configure and secure a Linux system using the **Uncomplicated Firewall (UFW)**. The goal was to define default firewall policies, allow specific services, enable logging, and verify system connectivity through testing and troubleshooting.

---

## **Objectives**
1. Understand the purpose and functionality of the **Linux Uncomplicated Firewall (UFW)**.  
2. Gain hands-on experience installing, enabling, and managing UFW.  
3. Configure default rules to control network traffic effectively.  
4. Enable logging to monitor firewall activity.  
5. Verify rule functionality through testing and troubleshooting.

---

## **Scenario**
You are a **junior system administrator** managing a Linux server experiencing intermittent connectivity issues.  
Your task is to implement proper firewall rules using UFW to secure the server, test connectivity, and confirm logging is active.

---

## ⚙️ **Step 1: Verify Firewall Status**

Before configuring, verify whether UFW is active and check existing rules.

<span style="color:#00bfff">**Command:**</span>  
`sudo ufw status`

<span style="color:#98fb98">**Output Example:**</span>  
```
Status: active
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
Anywhere                   DENY        192.168.1.100
22/tcp (v6)                ALLOW       Anywhere (v6)
```

<span style="color:#ffd700">**Explanation:**</span>  
- <span style="color:#ff6347">22/tcp</span> represents SSH access.  
- <span style="color:#9370db">Anywhere (v6)</span> entries represent IPv6 rules.  
- <span style="color:#00fa9a">Anywhere</span> (without v6) applies to IPv4.

---

## 🔒 **Step 2: Configure Default Policies**

Set the default behavior for incoming and outgoing traffic to establish a secure baseline.

<span style="color:#00bfff">**Commands:**</span>  
`sudo ufw default deny incoming`  
`sudo ufw default allow outgoing`

<span style="color:#ffd700">**Explanation:**</span>  
- <span style="color:#ff6347">deny incoming</span> → blocks all inbound traffic unless explicitly allowed.  
- <span style="color:#00fa9a">allow outgoing</span> → permits all outbound traffic from your system.  
- This enforces a **least-privilege security model**.

---

## 🌐 **Step 3: Allow Specific Services**

Allow necessary network services using either service names or port numbers.

<span style="color:#00bfff">**Commands:**</span>  
`sudo ufw allow http`  
`sudo ufw allow https`  
`sudo ufw allow 80`  
`sudo ufw allow 443`

<span style="color:#ffd700">**Explanation:**</span>  
Both `sudo ufw allow http` and `sudo ufw allow 80` open **port 80/tcp** for web traffic, but running both commands creates duplicates:

```
80/tcp   ALLOW   Anywhere
80       ALLOW   Anywhere
80/tcp (v6) ALLOW Anywhere (v6)
80 (v6)  ALLOW   Anywhere (v6)
```

To clean up redundant rules and keep the configuration efficient, use:

`sudo ufw delete allow 80`

This removes duplicate entries and keeps only one active rule per port.

---

## 🧩 **Step 4: Verify Connectivity**

Test outbound connectivity by pinging Google’s public DNS server.

<span style="color:#00bfff">**Command:**</span>  
`ping -c 4 8.8.8.8`

<span style="color:#98fb98">**Output Example:**</span>  
```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=255 time=19.4 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=255 time=19.3 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=255 time=19.2 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=255 time=19.1 ms
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss
```

✅ **Success:** Ping replies confirm that outgoing traffic is allowed and the firewall rules are functioning correctly.  
⚠️ **Note:** If ping fails, ICMP packets may be blocked — you can allow them using a rule for **ICMP** traffic.

---

## 🧾 **Step 5: Enable Firewall Logging**

Enable UFW’s logging to monitor both allowed and denied traffic.

<span style="color:#00bfff">**Command:**</span>  
`sudo ufw logging on`

<span style="color:#ffd700">**Explanation:**</span>  
- Logs are stored at `/var/log/ufw.log`.  
- Use `sudo tail -f /var/log/ufw.log` to view logs in real time.  
- Disable logging anytime with `sudo ufw logging off`.

---

## 🧱 **Step 6: Verify Final Configuration**

After adjustments, confirm all active rules and default policies.

<span style="color:#00bfff">**Command:**</span>  
`sudo ufw status verbose`

<span style="color:#98fb98">**Output Example:**</span>  
```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
Anywhere                   DENY        192.168.1.100
22/tcp (v6)                ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)
```

<span style="color:#ffd700">**Explanation:**</span>  
- **IPv4 Rules** appear as <span style="color:#00fa9a">Anywhere</span>.  
- **IPv6 Rules** appear as <span style="color:#9370db">Anywhere (v6)</span>.  
- These are *not* duplicates — they represent different network protocols:  
  - IPv4 covers traditional addresses like `192.168.1.10`.  
  - IPv6 covers newer address formats like `2001:db8::1`.  
- Both ensure the firewall protects *all* IP traffic.

Even though `22/tcp` appears twice, the `(v6)` version applies specifically to IPv6 connections. Removing one could block certain remote access types.

---

## ⚙️ **Step 7: Managing Rules**

You can **add**, **deny**, or **remove** rules dynamically as needed:

`sudo ufw allow <port/service>`  
`sudo ufw deny <port/service>`  
`sudo ufw delete allow <port/service>`

**Examples:**  
- `sudo ufw deny 23/tcp` blocks Telnet traffic.  
- `sudo ufw delete allow http` removes the HTTP allow rule.

---

## 🧠 **Summary**

This project demonstrated:
- Setting **default firewall policies** to enforce a secure baseline.  
- Allowing **specific ports and services** (HTTP, HTTPS, SSH).  
- Understanding that both `sudo ufw allow http` and `sudo ufw allow 80` work — but create **duplicate rules** that should be cleaned for efficiency.  
- Differentiating between **IPv4 and IPv6** firewall entries.  
- Verifying functionality with **ping tests**.  
- Enabling and monitoring **UFW logging** for auditing and troubleshooting.

By completing this project, I gained practical experience in **Linux firewall administration**, **traffic control**, and **network security fundamentals**.
