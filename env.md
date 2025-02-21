# App vm (srv2025) 
Docker desktop  
Nginx web server container (Stateless webapp)
# Pentest vm (Kali)
# Client vm (win11)
# SIEM vm (win11) (splunk/ELK)

#### -Install vms and update software.
#### -After updates create snapshots of each vm.
#### -Create private virtual switch to isolate from host computer.

# Carry out attacks before hardening. Generate a report and then review before hardening.
Steps.

1. Recon
2. Scan
3. Manual Test
4. Exploit
5. Report
6. Harden

# Key Areas
HTTPS config: Disable SSLv2 and SSLv3, enforce TLS 1.2+, use strong ciphers (RSA, SHA256), enable HTTP Strict Transport Security (HSTS) to force HTTPS.   
HTTP methods: Disable DELETE, PUT, TRACE, POST.   
HTTP responses: Disable X-Frame-Options. prevents clickjacking. X-Content-Type-Options, prevent MIME sniffing   
DDoS: Rate limit from one IP address to avoid resource exhaustion.
Directory Traversal: Ensure users cannot reach sensitive information via paths ../../etc/passwd, set correct file permissions  
Server Tokens: By default nginx reveals its version in HTTP headers, hide server info   
Container: Install ufw and block ports except 80, 443  
VM firewall: Block all ports except 80, 443  
XSS attacks: Headers like X-Forwarded-For, User-Agent can be manipulated into an XSS payload  
Simulate ~300 users accessing app simultaneously 
Nginx Caching: Reduces server load  
Logs & Monitorin: Enable access logs, error logs   
MInimize base images, least privilege within container, AppArmor/SELinux

## Attacks
#### Injection attacks: SQL, command, ldap.  
Tools: Burp, OWASP ZAP  

#### Authentication & Authorization: weak authentication mechanisms, broken access control, session management issues.  
Tools: Burp, Hydra  

#### Cross-Site Scripting (XSS): check if input fields allow malicious scripts.  
Tools: Burp, OWASP ZAP,   

#### Cross-Site request Forgery (CSRF): check if app can be tricked into performing actions on behalf of an authenticated user.  
Tools: Burp  

#### File Upload Vulnerabilities: file upload vulnerabilities.  
Tools: Burp  

## Nginx 
#### Configuration: block unused ports, limit HTTP methods, use SSL properly  
Tools: Hydra, Burp  

#### Service Discovery: identify ports/services running.  
Tools: Nmap, Netcat  

#### Vulnerability Scanning: scan for known vulnerabilities  
Tools: OpenVas, Nessus, Nikto  

#### Denial of Service (DoS): test for DoS vulnerabilities that could overwhelm server.  
Tools: LOIC, Hping, custom DoS scripts  

#### SSL/TLS Testing: test for encryption  
Tools: SSL labs, OpenSSL, testssl.sh  

# Toolkit
#### Web Application
Burp, OWASP ZAP, Nikto, Wfuzz, AppArmor, SELinux

#### Network & Services
Nmap, Masscan, Hydra, Netcat, Snort

#### Vulnerability Scanners
OpenVAS, Nessus, Qualys

#### OSINT
Recon-ng, Shodan, theHarvester

# Hardening
Web application firewall (WAF): ModSecurity  
Fail2Ban  
Security Headers  
SSH password disabling  
Disable unused services/ports  
