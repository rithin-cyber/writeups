# HTB Redeemer — Redis Database Enumeration

## Objective
Identify an exposed Redis service and inspect the lab database
to understand the risk of unauthorised database access.

## Enumeration
- Tool: Nmap and redis-cli
- Port: 6379/tcp
- Service: Redis
- Commands used in the lab:
  - `nmap -sV <TARGET_IP>`
  - `redis-cli -h <TARGET_IP>`
- Finding: Record whether the lab's Redis service allowed
  access without appropriate authentication.

## Validation
- Connected to Redis in the authorised HTB lab.
- Inspected database information.
- Identified the available database and keys.
- Retrieved the challenge value required by the lab.

## Security Impact
An exposed or inadequately protected Redis instance may
allow unauthorised access to stored data.

## Remediation
- Restrict Redis to trusted networks.
- Configure suitable authentication and access controls.
- Avoid exposing Redis directly to the public internet.
- Apply least privilege and monitor access.

## Limitations
Only the Redis service and data accessible in the lab
were assessed.
