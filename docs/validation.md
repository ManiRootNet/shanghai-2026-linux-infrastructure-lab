# Validation

Evidence was captured on **26 October 2026** from the running VMware practice environment. UTC capture timestamps are stored with structured results.

## Automated assertion set

| Set | Passed | Coverage |
| --- | --- | --- |
| Service access and TLS | 39 / 39 | HTTPS, internal-site authorization, FTPS roles and jail, SMTP/IMAP roles, STARTTLS enforcement, LDAPS binds and disabled plaintext LDAP |
| DNS | 14 / 14 | Eleven expected A records, two 360-second TTL checks and the MX record |
| Directory backend and submission policy | 7 / 7 | Password-change propagation to FTP and mail, restoration of original password values and unauthenticated submission rejection |
| **Total** | **60 / 60** | Named assertions in the JSON evidence |

[Read every assertion and observed result](../evidence/validation-summary.json).

Negative tests distinguish protocol-level rejection from connection timeout. FTP transfer tests use a client that supports TLS session reuse.

## Network and remote-access drills

- Five simultaneous mixed HTTP/HTTPS connections established from one source; a sixth timed out.
- MAIN and DMZ could not initiate a new TCP session to the client network.
- Both VPN-group users received addresses within the requested pool. A non-member received AUTH_FAILED.
- Route inspection confirmed that the remote service and client networks used the tunnel.
- HTTPS was requested with the tunnel interface explicitly selected. A client-network SSH banner was also read through that route.
- Disconnect removed the VPN interface and temporary credential file.
- Strict reverse-path filtering remained enabled during successful VPN and transfer tests.

## Shaping and protection

A 6 MiB object downloaded at **237,252 bytes/s**. A 6 MiB upload reached the receiver at **234,718 bytes/s**, with the receiver confirming the complete byte count. These are application-payload measurements under a configured 2 Mbit/s policy.

A controlled scan produced a source entry with a five-minute timeout. The entry was absent after expiration. Five controlled SSH failures produced an iptables DROP ban; the test source was unbanned after the rule was observed.

Seven rapid ICMP probes received five replies, each with TTL 128.

## Storage and persistence

Three newly attached 1 GiB virtual disks form an mdadm RAID5 array. The array reports [UUU], uses ext4 and mounts at the NFS export path after a server reboot.

Both pre-existing sample files retained their original SHA-256 hashes. A client-side write/read probe succeeded and its temporary file was removed.

[Read post-reboot storage evidence](../evidence/storage-integrity.txt).

## Recovery

The controlled drill disabled forwarding, removed the public-DMZ route and stopped DNS and SSH protection. Ansible repaired all four drifts with no failed or unreachable hosts. A subsequent run reported zero changes.

The recovery project was committed to the internal Git service and replayed from a fresh checkout. [Fresh-checkout evidence](../evidence/recovery-replay.txt) records zero changes and zero failures. Evidence records the live drift-repair and repeat-run scope.

## Evidence handling

Public artifacts contain selected outcomes, measurements and architecture. Credentials, private keys, password hashes and the private remediation record remain outside this repository. Original exam documents remain in the local reference set.
