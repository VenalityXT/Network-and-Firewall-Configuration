# **Network and Firewall Configuration**

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

## **Step 1: Verify Firewall Status**

Before configuring, verify whether UFW is active and check existing rules.

**Command:**
```
sudo ufw status
```

**Output Example:**
```
Status: active
To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
Anywhere                   DENY        192.168.1.100
22/tcp (v6)                ALLOW       Anywhere (v6)
```

- **22/tcp** represents SSH access.  
- **Anywhere (v6)** entries represent IPv6 rules.  
- **Anywhere** (without v6) applies to IPv4.

---

## **Step 2: Configure Default Policies**

Set the default behavior for incoming and outgoing traffic.  
This ensures a secure baseline for your system.

**Commands:**
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

**Explanation:**
- **deny incoming** blocks all inbound traffic unless explicitly allowed.  
- **allow outgoing** permits all outbound traffic from your system.  

These default rules ensure a least-privilege model where only explicitly defined services are exposed.

---

## **Step 3: Allow Specific Services**

Allow specific ports and services required by your system or applications.

**Commands:**
```
sudo ufw allow http
sudo ufw allow https
sudo ufw allow 80
sudo ufw allow 443
```

**Explanation:**
- **http (port 80)** enables standard web traffic.  
- **https (port 443)** enables secure web traffic.  
- Both `sudo ufw allow http` and `sudo ufw allow 80` achieve the same result — opening port 80.  
  However, when both commands are run, **UFW creates duplicate entries** in the ruleset because it treats them as two separate additions.

When checking the status after adding both, you’ll see duplicates:
```
80/tcp      ALLOW   Anywhere
80          ALLOW   Anywhere
80/tcp (v6) ALLOW   Anywhere (v6)
80 (v6)     ALLOW   Anywhere (v6)
```

To maintain **efficiency and clarity**, unnecessary duplicates can be removed using the delete command:

**Command Example:**
```
sudo ufw delete allow 80
```

This leaves only one active rule for each port.  

---

## **Step 4: Verify Connectivity**

Test outbound network connectivity by pinging Google’s DNS server to confirm outbound traffic is permitted.

**Command:**
```
ping -c 4 8.8.8.8
```

**Output Example:**
```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=255 time=19.4 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=255 time=19.3 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=255 time=19.2 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=255 time=19.1 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss
```

**Explanation:**
- Successful replies confirm your **outgoing traffic rules** are working correctly.  
- If ping fails, you can create a new rule allowing **ICMP** traffic (the protocol used by `ping`).

---

## **Step 5: Enable Firewall Logging**

Enable logging to monitor allowed and denied connections.

**Command:**
```
sudo ufw logging on
```

**Explanation:**
- Enables UFW’s internal logging system.  
- Logs are stored at **/var/log/ufw.log** for review.  
- Logging can be disabled with `sudo ufw logging off` if not needed.

---

## **Step 6: Verify Final Configuration**

After all changes, review your configuration.

**Command:**
```
sudo ufw status verbose
```

**Output Example:**
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

**Explanation:**
- **IPv4 Rules** appear as `Anywhere`.  
- **IPv6 Rules** appear as `Anywhere (v6)`.  
- These are not duplicates — they represent separate network protocols.  
  - IPv4 handles traditional address formats like `192.168.1.10`.  
  - IPv6 handles newer formats like `2001:db8::1`.  
- Both rule sets ensure your firewall protects all address families.

Even though `22/tcp` appears twice (once with and once without v6), each applies to a different protocol. Removing one could unintentionally block access from IPv6 hosts.

---

## **Step 7: Managing Rules**

You can **add**, **deny**, or **remove** rules dynamically:

```
sudo ufw allow <port/service>
sudo ufw deny <port/service>
sudo ufw delete allow <port/service>
```

**Examples:**
- `sudo ufw deny 23/tcp` blocks Telnet traffic.  
- `sudo ufw delete allow http` removes the HTTP allow rule.  

These commands provide flexibility to modify firewall behavior without disabling it.

---

## **Summary**

This project demonstrated:
- Setting **default firewall policies** to enforce a secure baseline.  
- Allowing **specific ports and services** (HTTP, HTTPS, SSH).  
- Understanding the relationship between **service names** and **port numbers**.  
- Identifying and removing **duplicate rules** for efficiency.  
- Differentiating between **IPv4 and IPv6** firewall entries.  
- Verifying functionality with **ping tests** and enabling **UFW logging** for audit visibility.

Through these configurations, I successfully secured a Linux system using UFW, ensuring both network accessibility and protection.


$${\color{red}This \space is \space red \space text.}$$
