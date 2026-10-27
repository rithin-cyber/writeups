HTB Bashed — Web Enumeration and Linux Privilege Escalation
Objective
Enumerate the exposed web server, identify the intended initial-access path, and investigate local permissions and scheduled tasks for a route from a web-service account to root.
Enumeration
- Tools: Nmap, Gobuster or Feroxbuster, browser, Linux shell enumeration tools
- Relevant service: HTTP on 80/tcp.
- Useful commands:
  - nmap -p- --open -T4 <TARGET_IP>
  - nmap -sC -sV -p 80 <TARGET_IP>
  - gobuster dir -u http://<TARGET_IP>/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,txt
- Finding: Directory enumeration identifies an exposed development area under /dev/ containing phpbash.php, a browser-accessible PHP command shell.
Initial Access
- Opening the exposed PHP Bash page permits command execution in the context of the web server, typically www-data.
- If an interactive shell is needed for further enumeration, use an authorised lab-only reverse-shell method and configure the listener using the reachable HTB VPN interface address.
- Validation commands:
  - whoami
  - id
  - hostname
- Finding: An exposed development shell provides an unauthenticated route to operating-system command execution.
Privilege Escalation
- Tool/command: sudo -l
- Finding: The www-data account can run commands as scriptmanager without a password.
- Switch user for enumeration:
  - sudo -u scriptmanager /bin/bash
- Inspect the unusual /scripts directory:
  - ls -la /scripts
  - cat /scripts/test.py
  - stat /scripts/test.txt
- Finding: test.py is writable by scriptmanager, while test.txt is owned by root and is refreshed by a scheduled process. This indicates that a root-owned scheduled task executes the writable Python script.
- In the lab, modifying the script allows code to execute with root privileges when the scheduled task runs. Validate with id and retrieve the root flag from /root/root.txt; retrieve the user flag from the user's home directory shown on the target.
Security Impact
An exposed web shell permits remote command execution. A permissive sudo rule allows a web-service account to become scriptmanager, and a root scheduled task that executes a lower-privileged user's writable script enables escalation to root.
Remediation
- Remove development shells such as phpbash.php from production web roots.
- Restrict web application permissions and prevent the web-service account from executing arbitrary commands.
- Remove unnecessary sudo permissions, especially unrestricted command execution as another account.
- Ensure scripts executed by root are owned by root and cannot be modified by unprivileged users.
- Audit scheduled tasks and monitor changes to their scripts and working directories.
Limitations
The findings describe the intentionally vulnerable HTB Bashed lab. The scheduled-task behaviour and access controls must be verified on the specific instance in use.
