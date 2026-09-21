# IT Infrastructure Home Lab

A hands-on IT infrastructure lab built using VirtualBox, Windows Server, Windows 11, Ubuntu Server, Active Directory, Group Policy, networking, file services, and SSH.

The purpose of this project is to develop practical **IT support, systems administration, networking, Linux, and troubleshooting skills** through a simulated enterprise environment.

---

## Lab Status

| Phase                                  | Status     |
| -------------------------------------- | ---------- |
| Phase 1 — Infrastructure Foundation    | ✅ Complete |
| Phase 2 — Windows Administration       | ✅ Complete |
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


# Phase 2 — Windows Administration

Phase 2 focused on developing practical Windows system administration and
helpdesk troubleshooting skills in a domain environment.

The objective was to simulate common tasks performed by an IT Support
Technician or Junior Systems Administrator, including user administration,
access control, Group Policy, file sharing, remote support, and
troubleshooting.

## Active Directory Administration

Using the Windows Server 2022 Domain Controller (DC01), I performed
day-to-day Active Directory administrative tasks including:

- Created and managed domain user accounts
- Created Organizational Units (OUs) for departments
- Created and managed security groups
- Added and removed users from departmental groups
- Reset user passwords
- Disabled user accounts
- Practised user onboarding and offboarding
- Used group membership to control access to network resources

This provided practical experience with identity and access management in
a Windows domain environment.

## Group Policy Administration

Configured and tested Group Policy Objects (GPOs) to centrally manage
domain users and computers.

Tasks included:

- Creating and linking GPOs
- Linking policies to appropriate Organizational Units
- Configuring departmental network-drive mappings
- Applying security filtering using Active Directory security groups
- Testing policies using domain user accounts
- Updating policies using `gpupdate /force`
- Troubleshooting policy application using `gpresult`

## File Sharing and Permissions

Created departmental shared folders and configured access using both SMB
share permissions and NTFS permissions.

Implemented role-based access control using Active Directory security
groups so that users could access only the resources required by their
department.

Tasks included:

- Creating SMB network shares
- Configuring NTFS permissions
- Assigning permissions through security groups
- Testing access with different domain users
- Troubleshooting "Access Denied" scenarios
- Mapping departmental network drives using Group Policy

## Troubleshooting Case Study — Network Drives Not Mapping

During testing, departmental network drives failed to map automatically
for some domain users.

### Symptoms

Group Policy Results showed the affected drive-mapping GPOs as:

`Not Applied (Unknown Reason)`

The client computer could still:

- Contact the domain controller
- Resolve DC01 through DNS
- Access SYSVOL and NETLOGON
- Successfully run `gpupdate`

This indicated that basic domain connectivity was functioning correctly.

### Investigation

I used tools including:

- `gpresult`
- `gpupdate`
- `ping`
- DNS/network connectivity checks
- Active Directory Users and Computers
- Group Policy Management

The issue was traced to Group Policy read/processing permissions.

The computer account did not have the required ability to read the GPO
files during policy processing.

### Resolution

I updated GPO delegation to grant **Authenticated Users** Read permission
while retaining the appropriate departmental security groups for
Security Filtering.

I then verified that each departmental GPO was linked to the correct OU,
forced a Group Policy update on CLIENT01, logged in with test domain
accounts, and confirmed that the appropriate departmental network drives
mapped successfully.

### Skills Demonstrated

This incident provided practical troubleshooting experience involving:

- Active Directory
- Group Policy
- GPO delegation
- Security filtering
- SYSVOL
- DNS
- SMB
- NTFS permissions
- Role-Based Access Control (RBAC)
- Windows client troubleshooting

Rather than immediately changing configurations, I worked through the
problem systematically by confirming connectivity, checking policy
results, identifying the failed component, implementing the fix, and
verifying the result.

## Remote Administration

Configured and tested remote administration between systems in the lab.

Tasks included:

- Remote Desktop Protocol (RDP)
- Windows remote administration
- AnyDesk remote-support testing
- Testing connectivity before initiating remote sessions
- Practising remote troubleshooting from a technician perspective

## Phase 2 Outcome

By completing Phase 2, I gained practical experience administering and
troubleshooting a Windows domain environment.

The phase strengthened my understanding of how Active Directory,
Group Policy, DNS, SMB, NTFS permissions, security groups, and Windows
clients work together in an enterprise environment.

These exercises were designed to simulate common responsibilities
encountered in IT Helpdesk, Desktop Support, and Junior Systems
Administration roles.

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
