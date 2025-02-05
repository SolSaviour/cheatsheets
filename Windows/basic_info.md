# Basic Information

## Room: [Intro 2 Windows](https://tryhackme.com/room/intro2windows)

### Permissions

**Logical Drives** - E.g. C Drive

Some important folders include:

| Folder Name | Importance |
| -- | -- |
| PerfLogs | Stores the system issues and other reports regarding performance |
| Program Files (x86) |  Is the location where programs install unless you change their path (E.g. Choosing to install software on D drive) |
| Windows | It's the folder which basically contains the code to run the operating system and some utility tools |

File Permissions...

| Permission | Explanation |
| -- | -- |
| Full Control |  allows the user/users/group/groups to set the ownership of the folder, set permission for others, modify, read, write, and execute files. |
| Modify | allows the user/users/group/groups to modify, read, write, and execute files. |
| Read & Execute | allows the user/users/group/groups to read and execute files. |
| List Folder Contents | allows the user/users/group/groups to list the contents (files, subfolders, etc) of a folder. |
| Read | only allows the user/users/group/groups to read files. |
| Write | allows the user/users/group/groups to write data to the specified folder (automatically set when "Modify" right is checked). |

### Directory Authentication

**LSA** - Local Security Authentication is a protected subsystem that keeps track of security policies and accounts on a Windows system. Below discusses a subcomponent of LSA, Active Directories.

**ACTIVE DIRECTORY (AD):** A directory (i.e. database) service for *Windows domain networks* which is essentially the database + set of services that connect users with network resources. The database (or "directory") contains critical information about your environment, including what users and computers there are and who's allowed to do what. Below discusses the types of ADs...

**1. On-Premise AD (AD)** - this environment has a record of all users, PCs, and servers and authenticates the users signing in. Once signed in, Active Directory also governs what the users are, and are not, allowed to do or access (authorization). It uses the following auth. protocols:

| Auth. Protocol | Description |
| -- | -- |
| NTLM (New Technology LAN Manager) | Uses challenge-response authentication scheme and does NOT provide data integrity or confidentiality protection for the authenticated connection. |
| LDAP / LDAPS (Lightweight Directory Access Protocol - Secure) | LDAP is an open, vendor-neutral application protocol for accessing and maintaining usernames, passwords, email addresses, printer connections, and other static data. |
| KERBEROS | Uses symmetric-key crypto and requires TTP (A Key Distribution Centre - which often runs on Domain Controllers as part of AD Domain Services) authorization to verify user identities. |

*The difference between NTLM and Kerberos* - "While NTLM uses a three way handshake between the client and server, where credentials are sent between the systems, Kerberos avoids sending credentials across the network."

**2. Azure AD (AAD)** - A separate online authentication store (cloud) containing user and group credentials. It uses the following auth. protocols:

| Auth. Protocol | Description |
| -- | -- |
| SAML (Security Assertion Markup Language) | SSO (single sign-on) standard which defines a set of rules to allow users to access web apps (known as "Service Providers") via a single login. This is possible as service providers trust identity providers (perform authentication). |
| OAUTH 2.0 | A standard used by apps to provide access to client apps. Read more [here](https://datatracker.ietf.org/doc/html/rfc6749#section-1). |
| OpenID Connect | Built on top of OAuth 2.0, it adds an additional "ID Token" using JSON Web Tokens (JWT). |

> While OAuth 2.0 is about resource access and sharing, OIDC is all about user authentication.

### Utility Tools

| Tool | Description |
| -- | -- |
| Task Scheduler | Allows predefined actions to be automatically executed whenever a certain set of conditions is met. |
| Event Viewer | Logs events that happen across the device (Ex: Successful & Failed login attempts, System Errors, etc). |
| Shared Folders | a directory or a folder that can be shared across the network and can be accessed by multiple users. |
| Local Users & Computers | Using local users and computers we can create users, add them to different built-in groups, and they can be given different levels of access. |
| Performance Monitor | monitors the different activities across the device such as CPU usage, memory usage, etc. |
| Disk Management | shrink, expand, create new partitions (drives) and format the partitions. |
| Services & Applications | check the running services on the system and you have the ability to start, stop or restart them. |

**Local Security Policy:** a group of settings you can configure to strengthen the computer's security. You can set the minimum password length, the password complexity level, you can disable guest & local administrator accounts, and many more.

> **Note:** If the computer is not integrated into an Active Directory environment disabling local administrator account is a bad idea.

**Registry Editor:** The Windows registry database stores many important operating system settings. For example, it contains entries with information about what should happen when double-clicking a particular file type or how wide the taskbar should be. *To Access:* **Windows Key + R** => **RegEdit**

*CMD vs Powershell* - Powershell is the new and improved Microsoft shell that introduces a lot of new functionality such as piping.

### Servers

| Type | Description |
| -- | -- |
| Domain Controller | Responds to authentication requests and verifies users on computer networks. Domains are a hierarchical way of organizing users and computers that work together on the same network. The domain controller keeps all of that data organized and secured. [src](https://www.varonis.com/blog/domain-controller/) |
| File | Provide a great way to share files across devices on a network. |
| Web | Serves static or dynamic content to a Web browser by loading a file from a disk and serving it across the network to a user’s Web browser. |
| FTP | Makes possible moving one or more files securely between computers while providing file security and organization as well as transfer control. [FTP vs File](https://www.ftptoday.com/blog/the-difference-between-a-ftp-server-and-a-file-server) |
| Mail | Move and store mail over corporate networks (via LANs and WANs) and across the Internet. |
| Database | A computer system that provides other computers with services related to accessing and retrieving data from one or multiple databases. |
| Proxy | Sits between a client program and an external server to filter requests, improve performance, and share connections. |
| Application | Used to connect the database servers and the users. |

*AD vs DC* - Active Directory is a type of domain, and a domain controller is an important server on that domain. Kind of like how there are many types of cars, and every car needs an engine to operate. Every domain has a DC, but not every domain is AD.

**GPO** - Group Policy Object is a feature of Active Directory that adds additional controls to user accounts and computers.