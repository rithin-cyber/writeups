# HTB Meow — Telnet Service Security

## Objective
Enumerate the target and gain access to the lab environment
using the weakness provided by the challenge.

## Enumeration
- Tool: Nmap
- Port: 23/tcp
- Service: Telnet
- Command: `nmap -sV <TARGET_IP>`
- Finding: Telnet was exposed on the target.

## Validation
- Connected to the Telnet service in the authorised HTB lab.
- Tested the lab's permitted login scenario.
- Obtained access and located `flag.txt`.

## Security Impact
Telnet transmits data without modern transport encryption.
Weak or improperly configured authentication may allow
unauthorised access to the system.

## Remediation
- Replace Telnet with SSH.
- Disable weak or default credentials.
- Restrict remote access to authorised users and networks.
- Apply least-privilege permissions.

## Limitations
Only the services and access conditions provided by the
HTB lab were tested.
