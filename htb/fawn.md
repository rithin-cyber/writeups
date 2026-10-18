# HTB Fawn — FTP Anonymous Access

## Objective
Enumerate the target and retrieve the lab flag.

## Enumeration
- Tool: Nmap
- Port: 21/tcp
- Service: FTP
- Version: vsftpd 3.0.3
- Finding: Anonymous FTP login permitted.
- Command: Add the exact Nmap command you used.
- Evidence: Record the relevant Nmap output showing
  the FTP service and anonymous-access finding.

## Validation
- Connected to the FTP service.
- Logged in using the anonymous account.
- Listed the available files.
- Retrieved `flag.txt` from the authorised lab.

## Security Impact
Anonymous users may access files exposed through the FTP
service, potentially disclosing sensitive information.

## Remediation
- Disable anonymous access unless explicitly required.
- Restrict FTP access and file permissions to authorised users.
- Prefer SFTP or another appropriately secured transfer method.

## Limitations
Only the exposed FTP service and the accessible file were
validated during this exercise.
