## **Open Service Ports Using Kali Linux**

#### **Objective**
To improve Structureality Inc.’s security posture by identifying open service ports, enumerating service identities, and identifying the operating systems of systems in different network environments.

#### **Prerequisites**
1. **Kali Linux** installed and updated.
2. A target system/network to scan.
3. Administrative permissions (if needed) for specific networks.
4. Tools installed on Kali Linux:
   - `Nmap` (Network Mapper)
   - `Netcat` (for manual port enumeration)
   - `Nikto` (optional, for web server analysis)
5. Documentation software for reporting findings (e.g., LibreOffice, Markdown editor).

---

### **Lab Setup**

1. **Network Environment Preparation:**
   - Set up three network zones to mimic real-world environments:
     - **Border Firewall**: The external perimeter of the organization's network.
     - **Guest Network**: A network for unauthenticated or limited access users.
     - **Client Network**: Internal network for authenticated users and servers.

2. **Ensure Target Systems Are Running:**
   - Set up virtual machines or devices with known services (e.g., Apache, MySQL, SSH) to emulate realistic environments for scanning.

3. **Lab Rules:**
   - Avoid intrusive scans on unauthorized systems.
   - Inform team members and follow your organization's security policies.

---

### **Steps**

#### **Step 1: Evaluate the Border Firewall**

1. **Objective:**
   - Identify open service ports exposed to the internet.

2. **Commands:**
   - Use `Nmap` for port scanning.
     ```bash
     nmap -sS -p- -Pn <target IP>
     ```
     - `-sS`: TCP SYN scan for stealth.
     - `-p-`: Scan all 65,535 ports.
     - `-Pn`: Disable ping check (useful if ICMP is blocked).

   - If you suspect service banners are hidden, use a service version scan:
     ```bash
     nmap -sV <target IP>
     ```
     - `-sV`: Detect service versions.

   - Check for operating system information:
     ```bash
     nmap -O <target IP>
     ```
     - `-O`: Enable OS detection.

3. **Analysis:**
   - Document all open ports and their associated services.
   - Identify if any exposed services are unnecessary or misconfigured.

4. **Actions:**
   - Share findings with the firewall administrator to close unnecessary ports.

---

#### **Step 2: Analyze Scan Results from the Guest Network**

1. **Objective:**
   - Determine if unauthorized devices on the guest network expose services or sensitive information.

2. **Commands:**
   - Scan the subnet used by guest devices:
     ```bash
     nmap -sn <subnet>
     ```
     - `-sn`: Host discovery only.

   - For active hosts, perform a port scan:
     ```bash
     nmap -sS -p 1-1000 <active host IP>
     ```
     - Scan the first 1,000 commonly used ports.

   - Check for additional details, such as scripts for vulnerability detection:
     ```bash
     nmap --script vuln <active host IP>
     ```
     - `--script vuln`: Use vulnerability scripts.

3. **Analysis:**
   - Review open ports and services for security implications.
   - Identify devices that might be inadvertently sharing resources (e.g., printers, file shares).

4. **Actions:**
   - Coordinate with IT to isolate or secure devices exposing services unnecessarily.

---

#### **Step 3: Scrutinize the Server from the Client Network**

1. **Objective:**
   - Perform a deep analysis of internal servers for open ports and exposed services.

2. **Commands:**
   - Conduct a detailed scan of server IP:
     ```bash
     nmap -A -p- <server IP>
     ```
     - `-A`: Aggressive mode (OS detection + service version + traceroute).

   - Check for web server vulnerabilities (if applicable):
     ```bash
     nikto -h <server IP>
     ```
     - Use `Nikto` to identify misconfigurations or outdated software on web servers.

   - For a more manual approach, connect to specific ports:
     ```bash
     nc <server IP> <port>
     ```
     - `Netcat` can verify service banners.

3. **Analysis:**
   - Document high-risk services such as databases or remote management interfaces (e.g., RDP, SSH).
   - Highlight services running with outdated or vulnerable versions.

4. **Actions:**
   - Collaborate with the server administrator to patch vulnerabilities or limit access.

---

### **Post-Lab Reporting**

1. **Documentation:**
   - Create a comprehensive report detailing:
     - Network zone scanned.
     - List of open ports and services identified.
     - Risk assessment for each open service.
     - Recommendations to reduce the attack surface.

2. **Sample Table:**

   | Network Zone    | IP Address     | Open Ports | Services         | Risk Level | Recommendation             |
   |-----------------|----------------|------------|------------------|------------|----------------------------|
   | Border Firewall | 203.0.113.1    | 22, 80     | SSH, HTTP        | High       | Close SSH on public IP.    |
   | Guest Network   | 192.168.1.100  | 445        | SMB              | Medium     | Disable SMB sharing.       |
   | Client Network  | 10.0.0.200     | 3306       | MySQL            | High       | Restrict access to MySQL.  |

3. **Recommendations:**
   - Close unnecessary ports.
   - Update outdated software versions.
   - Implement firewall rules for internal and external network traffic.

---

### **Conclusion**
This lab demonstrates how to identify open ports and evaluate their risks.






## ❤️ Support My Work

If you find these projects helpful or inspiring, please consider supporting my work! Your contributions will help me dedicate more time to creating educational content and building more projects to share with the community.

### Ways to Support:
1. **Sponsor me on GitHub**  
   Visit my [GitHub Sponsors page](https://github.com/sponsors/CoderBlee/dashboard) to make a one-time or recurring donation. Every bit of support makes a big difference!

2. **Buy Me a Coffee**  
   Fuel my coding sessions by [buying me a coffee](https://buymeacoffee.com/gamu
   ). ☕ Your support keeps the inspiration flowing!

3. **Spread the Word**  
   - Share this repository with others who are learning cybersecurity.  
   - Follow me on [Twitter](https://x.com/ChiRai_rai) or [LinkedIn](https://www.linkedin.com/in/gamuchirai-blessing-muchafa/) for updates.  

4. **Contribute**  
   - Open an issue or submit a pull request to improve the projects.  
   - Suggest ideas for future projects or enhancements.  

### Thank You!
Your support means the world to me. Together, we can inspire more learners to dive into the exciting world of cybersecurity.
