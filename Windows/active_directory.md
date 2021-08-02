# Active Directory

## Room: [Active Directory Basics](https://tryhackme.com/room/activedirectorybasics)

**Why use an AD?** - It allows for control and monitoring of company user computers through a single domain controller. Any user can access their files in the server on any computer. 

**What is a Domain Controller?** - A Windows Server which has Active Directory Domain Services (AD DS) installed on it. They do a number of things:

* holds the AD DS data store - this is _NTDS.dit_, the database containing all user credentials + other DC info
* handles authentication + authorisation services

Some terminology:

| Term | Definition |
| -- | -- |
| Trees | A hierarchy of domains in Active Directory Domain Services |
| Domains | Used to group and manage objects |
| Organizational Units (OUs) | Containers for groups, computers, users, printers and other OUs |
| Trusts | Allows users to access resources in other domains |
| Objects | users, groups, printers, computers, shares |
| Domain Services | DNS Server, LLMNR, IPv6 |
| Domain Schema | Rules for object creation |

### Users

There are four types of users:

| User | Role |
| -- | -- |
| Domain Admins | This is the big boss: they control the domains and are the only ones with access to the domain controller. |
| Service Accounts (Can be Domain Admins) | These are for the most part never used except for service maintenance, they are required by Windows for services such as SQL to pair a service with a service account |
| Local Administrators | These users can make changes to local machines as an administrator and may even be able to control other normal users, but they cannot access the domain controller. |
| Domain Users | These are your everyday users. They can log in on the machines they have the authorization to access and may have local administrator rights to machines depending on the organization. |

### Groups

Makes user permission management easier. The two types of groups are:

1. Security Groups - specify permissions for a large number of users
2. Distribution Groups - specify email distribution lists

### Trusts vs Policies

**Trusts** - a mechanism foru users to gain access to other resources in the domain. It determines how the domains and trees communicate and send data. Often, abusing trust allows us to move laterally throughout the network.

**Policies** - rules for the domain to follow. E.g. create a group policy object disabling Windows Defender.