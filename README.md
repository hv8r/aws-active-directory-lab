# aws-active-directory-lab
AWS Active Directory &amp; Identity Management Lab

==============================================

Overview
--------
A self-hosted Active Directory Domain Services (AD DS) environment deployed on Amazon Web Services (AWS). This lab simulates a corporate identity management infrastructure, covering VPC network isolation, DNS resolution, Group Policy enforcement, and PowerShell user automation.

Architecture
------------
AWS VPC
├── Public Subnet (Internet Gateway)
└── Private Subnet
     ├── EC2: Windows Server 2022 (Primary Domain Controller - AD DS, DNS)
     └── EC2: Windows 10/11 Pro (Domain-Joined Client Workstation)

Technologies Used
-----------------
* Cloud: AWS EC2, VPC, Security Groups, Custom DHCP Option Sets
* Systems: Windows Server 2022, Windows 10/11 Pro
* Identity & Networking: Active Directory (AD DS), DNS, Group Policy (GPMC)
* Scripting: PowerShell, RSAT

Implementation Summary
----------------------
1. Cloud Provisioning: Configured an isolated AWS VPC, assigned a static private IP to a Windows Server 2022 EC2 instance, and restricted Security Group ports to RDP (3389), DNS (53), Kerberos (88), and LDAP (389).
2. Domain Deployment: Installed AD DS and DNS roles, promoted the server to Domain Controller for the root domain (corp.local), and configured external DNS forwarders.
3. Directory Structure: Built departmental Organizational Units (IT, HR, Finance, Workstations, Servers) and automated account creation using PowerShell.
4. Policy Enforcement: Linked GPOs to enforce 12-character password complexity, account lockout thresholds, and automatic 15-minute workstation screen locks.

Troubleshooting Highlights
--------------------------
* Domain Join DNS Failure: Client workstations initially failed to locate the domain controller because default AWS DHCP settings pointed to Amazon DNS. Resolved by creating a custom VPC DHCP Option Set routing DNS queries directly to the Domain Controller's private IP.
* Authentication Timeouts: Audited and updated AWS Security Group and Windows Defender Firewall rules to permit internal Kerberos and LDAP traffic between instances.

Repository Structure
--------------------
* scripts/bulk_user_provisioning.ps1 - Automates Active Directory user creation and OU assignment.
* docs/security_baselines.md - Documents domain password and workstation GPO settings.
