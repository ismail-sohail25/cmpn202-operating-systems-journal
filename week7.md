[⬅️ Week 6 – Performance Evaluation](week6.md) | [🏠 Home](README.md) | 
---

# **Week 7- Security Audit Report and System Evaluation:**

## **Lynis Security Audit:**
To be able to perform a comprehensive infrastructure security assessment, the Lynis auditing tool was used. Lynis is a security auditing tool designed to scan for misconfigurations, hardening opportunities and compliance issues.  
The following command was executed on the server via SSH:  
• sudo lynis audit system  

### **Lynis Results- Initial Scan:**
The initial Lynis scan reported a hardening index score of 61, showing that although security controls were in place there are still opportunities for improvement. The audit identified areas related to kernel parameters, system services and file permissions that could be improved further.  

`images/week7-lynis-initial-scan.png`

### **Lynis Remediation Actions:**
Following the audit recommendations, the system was reviewed to ensure that:  
• SSH hardening was applied correctly(key-based authentication)  
• Firewall rules were restrictive  
• Automatic security updates were enabled  
• AppArmor profiles were enforced  
• Fail2Ban was active and protecting SSH  

After checking and applying recommended configurations, the Lynis audit was executed again using the same command.  
• sudo lynis audit system  

### **Lynis Results- Post Remediation Scan:**
After remediation, the lynis hardening score improved, demonstrating security enhancement. Although the score did not increase much the increase still confirms that security controls were correctly implemented and validated  

`images/week7-lynis-post-remediation.png`

---

## **Network Security Assessment (Nmap):**
To evaluate the network exposure of the server, a network security scan was performed using nmap from the workstation.  
The following command was used:  
• nmap -Pn 192.168.100.11  

The -Pn was used because ICMP ping requests were blocked by the firewall.  

### **Nmap Results:**
The scan confirmed:  
• Port 22 (SSH) was open  
• All others ports were closed/filtered  
• No unnecessary services were exposed  

This confirms that the firewall configuration from previous weeks successfully restricted the attack surface.  

`images/week7-nmap-scan-results.png`

---

## **SSH Security Verification:**
To make sure that SSH remained securely configured, the SSH service status was checked on the server.  
To do this the following command was executed:  
• sudo systemctl status ssh  

The output confirmed that the SSH service was active and running.  
Also, AppArmor enforcement was verified to ensure mandatory access control was applied. The following command was used  
• sudo aa-status  

The output showed multiple profiles loaded and enforced, confirming that AppArmor was actively protecting system services  

`images/week7-ssh-status.png`  
`images/week7-apparmor-status.png`

---

## **Service Inventory and Justification:**
A service audit was conducted to identify all running services on the server an justify their necessity.  
The following command was used:  
• systemctl list-units –type=service –state=running  

`images/week7-running-services-audit.png`

---

## **System Configuration Review:**
A final system configuration review was conducted to make sure all security measures from weeks 4,5 and 6 were correctly applied.  
The review confirmed:  
• SSH key based authentication enforced  
• Firewall rules restricting SSH to the workstation IP  
• Fail2Ban was protecting against brute force attacks  
• AppArmor enforcing access control  
• Automatic security updates were enabled  
• Monitoring and baseline scripts are functioning correctly.  

All this demonstrates a consistent and well maintained security configuration.

---

## **Essential and Justified Services:**
• ssh.service – Required for secure remote administration from the workstation. SSH access is hardened using key-based authentication, root login disabled and firewall restrictions.  

• ufw – Controls inbound and outbound traffic. Only SSH access from the workstation IP is allowed. This service is important for network security and attack surface reduction.  

• fail2ban.service – Provides automated protection against brute force attacks by monitoring authentication logs and banning malicious IP addresses. It improves intrusion prevention  

• apparmor.service – Enforces mandatory access control policies that restrict application capabilities even if compromised. It reduces the impact of privilege escalation.  

• systemd-journald.service – Responsible for system and security logging. It is important for auditing monitoring and forensic analysis.  

• cron.service – This is used for scheduled tasks including automatic security updates. It supports system maintenance and security patching  

• unattended-upgrades.service – This makes sure that critical security updates are automatically installed. This reduces the exposure to vulnerabilities.  

• systemd-networkd.service / systemd-resolved.service / systemd-timesyncd.service – This allows core networking and time synchronisation required for stable system operation.  

• udisks2.service / systemd-udevd.service / multipathd.service – Handles disks and device management.  

• polkit.service – This manages authorisation for privileged operations. This is important for controlled administrative actions.  

---

## **Application and Temporary Services:**
• apache2.service – This is used for performance testing and web service evaluation. While important for testing this service should be disabled.  

• iperf3.service – This is temporarily enabled for network performance testing. This service should not remain running permanently.  

All active services were reviewed. No unnecessary or unknown services were found running.

---

## **Remaining Risk Assessment:**
Despite implementing strong security controls throughout the coursework some risks still remained.  
• Zero-day vulnerabilities – Even with automatic updates enabled, newly discovered vulnerabilities may exist before patches are released.  

• Insider Threats – Authorised users with administrative privileges could misue access. This risk can be mitigated through least-privilege principles, sudo logging and strong authentication.  

• Denial of Service Attacks – While firewall rules and Fail2Ban mitigate brute force attacks, high volume traffic floods could impact availability.  

• Application Level Vulnerabilities – Services such as Apache may contain misconfigurations or application vulnerabilities if beyond testing purposes.  

Overall the system demonstrates a low to moderate risk profile. Strong hardening measures such as SSH key authentication, firewall restrictions, AppArmor enforcements, Fail2Ban and automated updates reduce the likelihood of attacks.

---

## **Week 7 Reflection:**
Week 7 provided a complete overview of the server security through auditing and evaluation. Conducting a Lynis audit improved my understanding of security benchmarking. Networking scanning with Nmap confirmed that the server’s attack surface was minimal.  
Verifying SSH security, access control enforcement and running services reinforced the importance of layered security. Overall in this final week validated that the system was securely designed, correctly configured and suitable for real-world use.

---

[⬅️ Week 6 – Performance Evaluation](week6.md) | 
