### ** Finding Open Service Ports with Kali Linux**

#### **Objective**
1. Understand **common threat vectors and attack surfaces** through port and service scanning.
2. **Analyze reconnaissance results** to assess vulnerabilities and recommend mitigation strategies.

---

### **Scenario Recap**
You are tasked with discovering open service ports on systems in various network zones (border firewall, guest network, client network) using Kali Linux. This is to identify potential threat vectors and minimize attack surfaces. The lab emphasizes reconnaissance, analysis of findings, and actionable recommendations.

---

### **Detailed Lab Plan**

---

#### **Step 1: Evaluate the Border Firewall**  
##### **Objective: Explain Threat Vectors and Attack Surfaces**
- **Purpose**: Open ports on the border firewall may allow adversaries to access internal systems.
- **Threat Vector**: Internet-facing services (e.g., SSH, HTTP).
- **Attack Surface**: Any open ports visible externally.

##### **Procedure**:
1. **Run a full port scan on the border firewall:**
   ```bash
   nmap -sS -p- -Pn <firewall_public_IP>
   ```
   - `-sS`: Stealth SYN scan to identify open ports.
   - `-p-`: Scan all 65,535 ports.
   - `-Pn`: Skip ping; scan even if ICMP is blocked.

2. **Identify services running on open ports:**
   ```bash
   nmap -sV -p <open_ports> <firewall_public_IP>
   ```
   - `-sV`: Detect versions of services.

3. **Analyze OS and potential vulnerabilities:**
   ```bash
   nmap -O --script vuln <firewall_public_IP>
   ```
   - `-O`: OS detection.
   - `--script vuln`: Run vulnerability detection scripts.

##### **Analysis**:
- Document identified ports, services, and potential vulnerabilities.
- Example findings:
  - **Port 22 (SSH)**: Exposed service could allow brute force attacks.
  - **Port 80 (HTTP)**: Could expose outdated web server software.

##### **Recommendations**:
- Close non-essential ports.
- Restrict SSH access to trusted IPs.
- Update web server software or disable unnecessary services.

---

#### **Step 2: Analyze the Guest Network**  
##### **Objective: Identify Threat Vectors from Internal Reconnaissance**
- **Purpose**: Evaluate guest devices for unnecessary exposed ports or misconfigurations.
- **Threat Vector**: Unauthorized internal devices sharing sensitive resources.
- **Attack Surface**: Misconfigured devices accessible to other network users.

##### **Procedure**:
1. **Discover devices on the guest network:**
   ```bash
   nmap -sn <guest_network_subnet>
   ```
   - `-sn`: Host discovery (ping scan).

2. **Scan open ports on active devices:**
   ```bash
   nmap -sS -p 1-1000 <device_IP>
   ```

3. **Enumerate services and vulnerabilities:**
   ```bash
   nmap -A -p <open_ports> <device_IP>
   ```
   - `-A`: Aggressive scan mode (includes service version, OS detection).

##### **Analysis**:
- Example findings:
  - **Device A (192.168.1.100):**
    - Open Port 445 (SMB): May expose shared files.
    - Open Port 23 (Telnet): Unencrypted communication, prone to interception.

##### **Recommendations**:
- Disable file sharing (SMB) if not required.
- Replace Telnet with SSH for secure communications.

---

#### **Step 3: Scrutinize the Client Network Server**  
##### **Objective: Assess Reconnaissance Results to Reduce Attack Surface**
- **Purpose**: Deep analysis of internal servers to identify security gaps.
- **Threat Vector**: Exploitable services (e.g., databases, RDP).
- **Attack Surface**: Open ports that provide remote access.

##### **Procedure**:
1. **Conduct a comprehensive scan of the server:**
   ```bash
   nmap -A -p- <server_IP>
   ```
   - Scan all ports and collect detailed information about services and OS.

2. **Analyze web servers (if applicable):**
   ```bash
   nikto -h <server_IP>
   ```
   - Test for misconfigurations or outdated components.

3. **Manual inspection using Netcat:**
   ```bash
   nc <server_IP> <open_port>
   ```
   - Check service banners for sensitive information.

##### **Analysis**:
- Example findings:
  - **Open Port 3306 (MySQL)**: Database exposed, risking unauthorized access.
  - **Open Port 3389 (RDP)**: Remote access available; vulnerable to brute force.

##### **Recommendations**:
- Restrict database access to specific IP ranges.
- Enable two-factor authentication for RDP.

---

### **Post-Lab Activities**

1. **Create a Reconnaissance Report**  
   Include:
   - Identified open ports and services.
   - Potential vulnerabilities and threat vectors.
   - Recommendations to reduce attack surfaces.

   | Zone           | Target      | Open Ports | Services       | Risk Level | Recommendations                |
   |----------------|-------------|------------|----------------|------------|--------------------------------|
   | Border Firewall| 203.0.113.1 | 22, 80     | SSH, HTTP      | High       | Close SSH on public IP.        |
   | Guest Network  | 192.168.1.100 | 445, 23   | SMB, Telnet    | Medium     | Disable SMB, replace Telnet.   |
   | Client Network | 10.0.0.200  | 3306, 3389 | MySQL, RDP     | High       | Restrict MySQL, secure RDP.    |

2. **Correlate Findings with CompTIA Security+ Objectives**:
   - **2.2 Threat Vectors**: Highlight services or ports that can be exploited.
   - **2.3 Reconnaissance Analysis**: Discuss how scanning results indicate potential vulnerabilities.

3. **Action Plan**:
   - Prioritize closing unnecessary ports and securing essential services.
   - Implement monitoring to detect future unauthorized access.

---

### **Conclusion**
 The hands-on activities and analysis directly tie into CompTIA Security+ objectives, preparing you for real-world security assessments.