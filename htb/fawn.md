# HTB Fawn — Lab Notes

## Objective
Enumerate the target and retrieve the lab flag.

## Enumeration
- Tool: Nmap
- Port: 21/tcp
- Service: FTP
- Version: vsftpd 3.0.3
- Finding: Anonymous FTP login permitted
- Evidence: Nmap reported FTP anonymous login allowed.

## Validation
- Logged in using the anonymous FTP account.
- Listed the available files.
- Successfully retrieved flag.txt.

## Security Impact
Unauthenticated users may access files exposed through
the FTP service, potentially disclosing sensitive information.

## Remediation
Disable anonymous access unless explicitly required.
Restrict FTP access and file permissions to authorised users.
Prefer secure file-transfer protocols such as SFTP where appropriate.

## Limitation
Only the exposed FTP service and the accessible file were
validated in this exercise.
