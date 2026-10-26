# Engineering notes

## Test the complete authorization path

A service accepting a familiar password does not prove which identity backend it used. Directory tests temporarily changed a permitted user's password, checked rejection of the old value and acceptance of the new value, then restored the exact original stored value. FTP and mail both followed the directory change.

## Treat certificate extensions as service requirements

Matching names and SANs are part of certificate compatibility. OpenVPN also evaluates Key Usage and Extended Key Usage. The lab uses a one-year wildcard certificate with the required extensions, aligned trust stores and updated fingerprint pinning for remote logs.

## Include dynamic interfaces in routing policy

Tunnel and IFB devices appear after physical interfaces. Strict reverse-path validation is applied to them as well. VPN traffic and both directions of web shaping were retested with that setting active.

## Measure traffic policy at the application

A class declaring 2 Mbit/s provides configuration evidence. Timing a complete 6 MiB transfer adds behavioral evidence. Upload and download were measured separately; protocol overhead explains why payload rates fall below the configured link rate.

## Preserve data while changing storage

Existing NFS files were archived and hashed before mounting the RAID. The array was populated, re-exported, checked from a client and verified after reboot. Original sample files and messages remain available.

## Validate recovery convergence

Recovery was exercised against deliberate live drift, then repeated to check convergence. The internal Git history preserves the project and a fresh checkout was used for replay. Resolver recovery compares the active primary server so routine DHCP-generated comments do not create unnecessary changes.

## Tools used

Debian 13, VMware Workstation, nftables, iptables, OpenVPN, OpenLDAP, chrony, rsyslog, BIND, Apache, ProFTPD, Postfix, Dovecot, Docker, Prometheus, Grafana, mdadm, NFS, Ansible, Git, FileZilla and Thunderbird.
