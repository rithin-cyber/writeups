HTB Nibbles — Web Enumeration and Linux Privilege Escalation
Objective
Assess the exposed web application on the authorised HTB target, identify a viable path to initial access, and determine whether local permissions allow privilege escalation to root.
Enumeration
- Tools: Nmap, web browser, Gobuster or Feroxbuster, Burp Suite (optional)
- Relevant services observed on the standard machine: SSH on 22/tcp and HTTP on 80/tcp.
- Useful commands:
  - nmap -p- --open -T4 <TARGET_IP>
  - nmap -sC -sV -p 22,80 <TARGET_IP>
  - gobuster dir -u http://<TARGET_IP>/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
  - gobuster dir -u http://<TARGET_IP>/nibbleblog/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,txt
- Finding: The website exposes Nibbleblog. Reviewing the page source and enumerating paths identifies the /nibbleblog/ application, its administrative login page, and a README that identifies version 4.0.3.
Initial Access
- The lab's attack path uses access to the Nibbleblog administrator interface. The credentials reported for this machine are admin / nibbles; validate these against your own lab session rather than assuming they apply elsewhere.
- In the admin area, the My Image plugin accepts a PHP file where an image should be. Insufficient upload validation permits server-side PHP execution when the uploaded file is requested.
- A reverse shell or the relevant HTB exploit module can be used to obtain a shell in the web server's context.
- Finding: Initial access is possible because the application allows an authenticated user to upload executable PHP content.
Privilege Escalation
- Tool/command: sudo -l
- Finding: The nibbler account can run /home/nibbler/personal/stuff/monitor.sh as root without a password.
- The path is exploitable because the script can be created or modified by the current user in the lab's intended attack path. The script is then run through the permitted sudo rule.
- Validation commands:
  - id
  - sudo -l
  - sudo /home/nibbler/personal/stuff/monitor.sh
- Success condition: id confirms root-level privileges. Capture the user and root flags from the target itself; do not copy flag values from third-party writeups.
Security Impact
An outdated web application with weak administrator credentials and unsafe file-upload validation can lead to remote code execution. An overly permissive sudo rule then allows a foothold to become root access.
Remediation
- Upgrade or replace the outdated Nibbleblog version.
- Enforce strict server-side file type and content validation; store uploads outside executable web paths and disable script execution in upload directories.
- Use strong, unique administrator credentials and rate-limit authentication attempts.
- Remove unnecessary NOPASSWD sudo rules and ensure privileged scripts are owned and writable only by trusted administrators.
- Monitor web-server logs and administrative authentication events.
Limitations
This assessment concerns the intentionally vulnerable HTB lab only. Results depend on the version and configuration exposed by the instance being tested.
