# HTB Dancing — SMB Share Enumeration

## Objective
Enumerate SMB shares and verify whether unauthorised access
to files is possible in the authorised HTB lab.

## Enumeration
- Tool: Nmap and smbclient
- Port: 445/tcp
- Service: SMB
- Commands used in the lab:
  - `nmap -sV <TARGET_IP>`
  - `smbclient -L //<TARGET_IP>`
- Finding: Check the actual lab results for accessible shares
  and any guest or anonymous access.

## Validation
- Enumerated the available SMB shares.
- Tested access to the relevant share within the lab.
- Listed and retrieved the permitted challenge file.

## Security Impact
Poorly restricted SMB shares may expose sensitive files
or information to unauthorised users.

## Remediation
- Disable guest access where it is unnecessary.
- Restrict share and file permissions.
- Apply least privilege and limit network exposure.
- Review SMB configuration and access logs.

## Limitations
Only the shares and permissions available in the lab
were assessed.
