# IT Infrastructure Home Lab

A hands-on IT infrastructure lab built using VirtualBox, Windows Server, Windows 11, Ubuntu Server, Active Directory, Group Policy, networking, file services, and SSH.

The purpose of this project is to develop practical **IT support, systems administration, networking, Linux, and troubleshooting skills** through a simulated enterprise environment.

---

## Lab Status

| Phase                                  | Status     |
| -------------------------------------- | ---------- |
| Phase 1 — Infrastructure Foundation    | ✅ Complete |
| Phase 2 — Windows Administration       | 🔜 Planned |
| Phase 3 — Networking & Troubleshooting | 🔜 Planned |
| Phase 4 — Linux Administration         | 🔜 Planned |
| Phase 5 — Microsoft 365                | 🔜 Planned |
| Phase 6 — IT Service Desk & Ticketing  | 🔜 Planned |
| Phase 7 — Cloud & DevOps               | 🔜 Planned |

---

## Phase 1 — Infrastructure Foundation

Phase 1 focused on building the core infrastructure and establishing a working Windows and Linux environment.

### Virtual Machines

| Hostname | Operating System | IP Address      | Role                                  |
| -------- | ---------------- | --------------- | ------------------------------------- |
| DC01     | Windows Server   | `192.168.10.10` | Domain Controller / DNS / File Server |
| CLIENT01 | Windows 11       | `192.168.10.20` | Domain Client                         |
| ubuntu01 | Ubuntu Server    | `192.168.10.30` | Linux Server                          |

### Network

* **Network:** `Labnet`
* **Network type:** VirtualBox Internal Network
* **Subnet:** `192.168.10.0/24`

---

## Windows Server

DC01 was configured as the central Windows Server for the lab.

### Active Directory

* Active Directory Domain Services installed
* Domain: `homelab.local`
* Users created
* Security groups created
* Organizational Units configured
* Domain authentication tested

### Test Users

**IT**

* John Smith
* Security group: `IT`

**Sales**

* David Wilson
* Security group: `Sales`

---

## Group Policy

Group Policy Management was configured and tested.

Practical exercises included:

* Password policies
* Account lockout
* Account unlocking
* User and group management
* Drive mapping

A Sales network drive was mapped through Group Policy:

```text
\\DC01\Sales
```

The Sales drive was targeted to the Sales security group.

---

## File Server

A departmental Sales file share was created:

```text
\\DC01\Sales
```

Share and NTFS permissions were configured using Active Directory security groups.

### Access Testing

* David Wilson → Sales access ✅
* John Smith → Sales access denied ✅

This demonstrated role-based access control using Active Directory security groups.

---

## Ubuntu Server

Ubuntu Server was deployed as the Linux server component of the lab.

### Configuration

* Hostname: `ubuntu01`
* IP address: `192.168.10.30/24`
* Network interface: `enp0s3`
* Network: `Labnet`
* OpenSSH Server installed

### Connectivity Test

Ubuntu successfully communicated with DC01:

```bash
ping -c 4 192.168.10.10
```

Result:

```text
4 packets transmitted
4 packets received
0% packet loss
```

---

## SSH Remote Administration

SSH was enabled and verified:

```bash
sudo systemctl status ssh
```

The SSH service was confirmed as:

```text
Active: active (running)
```

Remote administration was then tested from CLIENT01:

```powershell
ssh thami@192.168.10.30
```

The remote session was verified using:

```bash
hostname
```

Result:

```text
ubuntu01
```

and:

```bash
whoami
```

Result:

```text
thami
```

---

## Troubleshooting

This project intentionally includes troubleshooting rather than only successful configurations.

Issues encountered during Phase 1 included:

* Windows 11 UEFI boot configuration
* Windows installation media and boot order
* VirtualBox networking
* Active Directory account lockout
* File share permissions
* Ubuntu NAT versus internal networking
* Ubuntu static IP configuration
* Ubuntu virtual disk detection
* Ubuntu login troubleshooting
* PXE/iPXE boot attempts
* SSH connectivity and verification

These incidents provided practical experience with identifying problems, investigating possible causes, applying fixes, and verifying the results.

---

## Skills Demonstrated

### Windows

* Windows Server administration
* Active Directory
* DNS
* Users and groups
* Organizational Units
* Group Policy
* Account lockout management
* File and folder permissions
* Network shares
* Domain administration

### Linux

* Ubuntu Server
* Linux command line
* Static IP configuration
* Network troubleshooting
* SSH
* `systemctl`
* `ip`
* `ping`
* Remote administration

### Networking

* IPv4 addressing
* `/24` subnet
* VirtualBox networking
* Internal networks
* NAT
* Client/server connectivity
* Network troubleshooting

### IT Support

* Incident investigation
* Troubleshooting
* Access control
* User account administration
* Documentation
* Verification and testing

---

## Project Roadmap

The lab will continue to expand into a broader simulated IT environment.

Future areas include:

* Advanced Windows Server administration
* Networking and troubleshooting
* Linux administration
* Microsoft 365 and Microsoft Entra ID
* IT service desk and ticketing
* Knowledge base documentation
* PowerShell
* AWS
* Terraform
* Docker
* CI/CD
* Cloud infrastructure
* DevOps automation

---

## Author

**Thamsanqa Faniso**

This project is a continuously evolving practical IT infrastructure lab designed to demonstrate hands-on technical skills and troubleshooting experience.
