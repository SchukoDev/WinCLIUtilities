# WinCLIUtilities

**Windows-style CLI utilities for Linux.**

WinCLIUtilities is a non-profit open-source project that provides Linux-compatible command-line utilities inspired by the behavior, syntax, workflows, and concepts of various utilities traditionally available on Microsoft Windows.

The project is designed for Linux users who are familiar with Windows command-line tools and want comparable workflows using native Linux mechanisms.

> **NON-PROFIT PROJECT**
>
> WinCLIUtilities is developed and distributed for educational, interoperability, experimentation, administration, and personal use.
>
> It is not affiliated with, endorsed by, sponsored by, or officially connected to Microsoft Corporation.

---

## Table of Contents

- [Overview](#overview)
- [Project Goals](#project-goals)
- [Compatibility Philosophy](#compatibility-philosophy)
- [Current Utilities](#current-utilities)
  - [ACCESSCHK](#accesschk)
  - [ADDUSERS](#addusers)
  - [ARP](#arp)
  - [ASSOC](#assoc)
  - [ATTRIB](#attrib)
  - [AUDITPOL](#auditpol)
- [Installation](#installation)
- [Requirements](#requirements)
- [Linux Distribution Compatibility](#linux-distribution-compatibility)
- [Privileges](#privileges)
- [Security Philosophy](#security-philosophy)
- [Data and Privacy](#data-and-privacy)
- [Windows and Microsoft Trademark Notice](#windows-and-microsoft-trademark-notice)
- [Relationship With Original Windows Utilities](#relationship-with-original-windows-utilities)
- [Compatibility Disclaimer](#compatibility-disclaimer)
- [Accuracy Disclaimer](#accuracy-disclaimer)
- [Administrative and Security Use](#administrative-and-security-use)
- [Backup Recommendation](#backup-recommendation)
- [Filesystem Compatibility](#filesystem-compatibility)
- [AUDITPOL Security Considerations](#auditpol-security-considerations)
- [Responsible Use](#responsible-use)
- [Open Source](#open-source)
- [Contributing](#contributing)
- [Pull Requests](#pull-requests)
- [Issue Reporting](#issue-reporting)
- [Third-Party Software](#third-party-software)
- [Licensing](#licensing)
- [Warranty Disclaimer](#warranty-disclaimer)
- [Limitation of Liability](#limitation-of-liability)
- [Project Status](#project-status)
- [Design Philosophy](#design-philosophy)
- [Roadmap](#roadmap)
- [Final Notice](#final-notice)

---

# Overview

Linux and Windows provide many utilities that solve similar administrative, diagnostic, networking, filesystem, and security problems.

However, their command-line interfaces, permission systems, filesystem models, networking stacks, process models, account systems, and security architectures are substantially different.

WinCLIUtilities attempts to bridge this gap by providing Windows-style command interfaces on Linux while relying, whenever possible, on the underlying Linux-native tools and security mechanisms.

The project does **not** attempt to recreate the Windows operating system.

Instead, it provides familiar command-line interfaces that translate or approximate Windows-oriented workflows into Linux-native operations.

---

# Project Goals

WinCLIUtilities aims to:

- provide familiar Windows-style commands on Linux;
- help Windows users transition to Linux;
- provide compatibility-oriented command-line workflows;
- expose Linux functionality through recognizable Windows-style syntax;
- demonstrate differences between Windows and Linux administration;
- provide educational examples of shell scripting and system administration;
- simplify administration for users familiar with Windows utilities;
- remain lightweight and transparent;
- use existing Linux utilities whenever possible;
- avoid unnecessary reimplementation of functionality already provided by Linux;
- clearly document differences between Windows and Linux behavior.

---

# Compatibility Philosophy

WinCLIUtilities follows the principle:

> **Familiar commands. Native Linux mechanisms. Honest compatibility.**

Linux and Windows are different operating systems.

Consequently, a Windows command and its WinCLIUtilities counterpart may have similar syntax and goals without having identical internal behavior.

There are four main compatibility categories.

## Native Equivalent

The Linux subsystem already provides a functionally similar mechanism.

Example:

```text
Windows ARP
    ↓
Linux ip neigh
````

## Translation

A Windows-oriented command is translated into one or more Linux operations.

Example:

```text
Windows file association
    ↓
Linux MIME type + .desktop application + mimeapps.list
```

## Approximation

A Windows feature has no direct Linux equivalent.

The project therefore uses the closest practical Linux mechanism.

Example:

```text
Windows file attributes
    ↓
Linux permissions + filesystem attributes + extended attributes
```

## Informational Compatibility

The command provides information using a Windows-style interface while the underlying data comes directly from Linux.

---

# Windows Concepts vs Linux Concepts

The following table illustrates some important differences:

| Windows concept              | Linux mechanism                                    |
| ---------------------------- | -------------------------------------------------- |
| NTFS ACL                     | POSIX/Linux ACL                                    |
| Windows Registry             | No universal Linux equivalent                      |
| Windows file attributes      | Permissions, filesystem flags, extended attributes |
| Windows Event Log            | systemd journal and other logging systems          |
| Windows Security Auditing    | Linux Audit (`auditd`)                             |
| Windows services             | systemd services                                   |
| Windows file associations    | MIME types and `.desktop` files                    |
| Windows networking           | Linux networking stack                             |
| Windows users                | Unix users / UID                                   |
| Windows groups               | Unix groups / GID                                  |
| Windows privileges           | Unix permissions, groups, capabilities, sudo       |
| Windows SIDs                 | Linux UID/GID and other identity mechanisms        |
| Windows security descriptors | Linux permissions, ACLs and capabilities           |

These mechanisms should not be considered interchangeable.

---

# Current Utilities

WinCLIUtilities currently targets Windows-style interfaces for several utilities.

---

# ACCESSCHK

`ACCESSCHK` provides a Windows-style interface for inspecting Linux permissions, ACLs, capabilities, processes, services, accounts, and network information.

It is inspired by the workflow and purpose of Microsoft's AccessChk utility, but it is implemented using Linux-native mechanisms.

## Examples

```bash
accesschk /?
accesschk file.txt
accesschk -r /etc
accesschk -w /home/user
accesschk -x /usr/bin
accesschk -world /
accesschk -priv
accesschk -caps /usr/bin
accesschk -process 1234
accesschk -service
accesschk -account
accesschk -network
accesschk -acl file.txt
```

Possible Linux mechanisms include:

```text
stat
ls
namei
getfacl
getcap
ps
systemctl
id
groups
ss
find
```

The command is intended to provide output that is understandable to users familiar with Windows AccessChk.

It should not be considered a binary or security-model-compatible implementation of Microsoft's original executable.

---

# ADDUSERS

`ADDUSERS` provides a Windows-style interface for Linux account and group administration.

The implementation relies on native Linux account-management utilities such as:

```text
useradd
usermod
userdel
groupadd
groupdel
chage
passwd
```

## Example

```bash
addusers
addusers users.csv
addusers -d users.csv
```

A supported CSV format may contain fields such as:

```text
Username,Full Name,Password,Description,Home,Shell,Groups
```

Linux and Windows account systems differ significantly.

Therefore, account creation, password expiration, password policies, account locking, group membership, and related operations are implemented according to Linux semantics.

The command should **not** be considered a drop-in replacement for Microsoft's original `ADDUSERS.exe`.

---

# ARP

`ARP` provides a Windows-style interface to the Linux network neighbor table.

## Examples

```bash
arp
arp -a
arp -g
arp -v
arp -d 192.168.1.10
arp -d "*"
arp -s 192.168.1.10 00-11-22-33-44-55
```

The implementation may rely on:

```bash
ip neigh
```

Linux uses a neighbor table rather than exactly reproducing the legacy Windows ARP subsystem.

MAC addresses may therefore be displayed using Linux conventions.

Administrative privileges may be required for operations that modify the neighbor table.

---

# ASSOC

`ASSOC` provides a Windows-style interface for file associations.

Linux generally handles file associations using:

* MIME types;
* `.desktop` files;
* XDG standards;
* `mimeapps.list`.

## Examples

```bash
assoc
assoc .pdf
assoc .txt
assoc .jpg
assoc .csv=text/plain
assoc .pdf=
assoc -l
assoc -v .pdf
assoc -a application/pdf
assoc -b associations.txt
assoc -r associations.txt
```

Conceptually:

```text
Windows:

.extension
    ↓
file type
    ↓
application
```

becomes approximately:

```text
Linux:

.extension
    ↓
MIME type
    ↓
.desktop application
```

These systems are not identical.

Consequently, removing a Linux association may remove a user-level default without removing the MIME definition itself.

---

# ATTRIB

`ATTRIB` provides a Windows-style interface for inspecting and modifying file attributes.

## Examples

```bash
attrib file.txt
attrib +R file.txt
attrib -R file.txt
attrib /S /D /R '*.txt'
```

Linux does not implement Windows file attributes in the same way.

Depending on the requested operation, WinCLIUtilities may use:

* Unix permission bits;
* filesystem attributes;
* `chattr`;
* `lsattr`;
* extended attributes;
* Linux filename conventions.

Linux filesystem flags can include:

```text
i   immutable
a   append-only
A   no atime updates
c   compressed
d   no dump
```

These flags are **not equivalent** to Windows attributes.

For example, a Linux hidden file is conventionally identified by its filename beginning with a period:

```text
.config
.bashrc
.local
```

There is no universal Linux equivalent of a Windows `H` attribute.

Some Windows-specific attributes therefore cannot be reproduced exactly.

---

# AUDITPOL

`AUDITPOL` provides a Windows-style interface to Linux auditing functionality.

The Linux implementation is based on components such as:

```text
auditd
auditctl
ausearch
aureport
augenrules
```

## Examples

```bash
auditpol /?
auditpol /get
auditpol /list /category
auditpol /list /subcategory
auditpol /backup /file:audit-backup.rules
auditpol /restore /file:audit-backup.rules
auditpol /clear /y
```

Linux Audit rules can monitor events such as:

* file access;
* file modifications;
* command execution;
* authentication;
* identity changes;
* privilege usage;
* network-related activity;
* kernel events;
* mount operations;
* system configuration changes.

A Linux Audit rule can look like:

```text
-w /etc/passwd -p wa -k identity
```

This watches a Linux resource using Linux Audit semantics.

It does not reproduce the Windows Security Auditing architecture.

## Important

Windows audit categories and Linux Audit rules are not one-to-one equivalents.

WinCLIUtilities may therefore provide a familiar interface while internally using Linux-specific audit concepts.

Audit configuration can have significant security and performance implications.

Some operations require root privileges.

---

# Installation

## Requirements

WinCLIUtilities primarily targets systems providing:

* Bash;
* standard GNU/Linux utilities;
* POSIX permissions;
* Linux ACL support where required;
* XDG utilities where required;
* Linux Audit where required.

On Debian, Ubuntu, and Kubuntu systems, the following packages may be useful:

```bash
sudo apt install acl attr xdg-utils auditd audispd-plugins
```

Additional functionality may require packages provided by the user's distribution.

---

# Installation From Source

Clone the repository:

```bash
git clone https://github.com/USERNAME/WinCLIUtilities.git
cd WinCLIUtilities
```

Follow the project's installation instructions for the current release.

If the utilities are implemented as shell functions loaded from `.bashrc`, reload the shell configuration after installation:

```bash
source ~/.bashrc
```

or:

```bash
. ~/.bashrc
```

---

# Shell Compatibility

WinCLIUtilities primarily targets:

```text
Bash
```

It is not guaranteed to work correctly under:

* `sh`;
* `dash`;
* `fish`;
* `csh`;
* other shells;

unless explicitly documented.

---

# Linux Distribution Compatibility

WinCLIUtilities is designed around standard Linux interfaces, but Linux distributions differ.

Potential differences may exist between:

* Debian;
* Ubuntu;
* Kubuntu;
* Linux Mint;
* Fedora;
* Arch Linux;
* openSUSE;
* RHEL;
* Rocky Linux;
* AlmaLinux;
* other distributions.

Differences can include:

* package names;
* filesystem locations;
* service configuration;
* default permissions;
* audit configuration;
* available utilities;
* default desktop associations.

Distribution-specific behavior should therefore be documented where necessary.

---

# Privileges

Some commands require elevated privileges.

Examples include operations involving:

* system users;
* system groups;
* protected files;
* network configuration;
* filesystem attributes;
* system services;
* Linux Audit;
* system-wide configuration.

The project intentionally respects Linux's existing privilege model.

If an underlying Linux command requires administrative privileges, the user must provide them through the normal Linux security mechanisms.

For example:

```bash
sudo auditctl -l
```

or, where supported by the implementation:

```bash
sudo auditpol /get
```

WinCLIUtilities does not attempt to bypass:

* `sudo`;
* Unix permissions;
* ACLs;
* Linux capabilities;
* mandatory access controls;
* authentication mechanisms.

---

# Security Philosophy

WinCLIUtilities is intended to operate through existing Linux security mechanisms.

The project does not intentionally attempt to:

* bypass authentication;
* bypass file permissions;
* circumvent `sudo`;
* obtain unauthorized privileges;
* exploit vulnerabilities;
* disable mandatory security controls;
* circumvent security policies;
* evade authorized monitoring.

The project is intended as an administration, interoperability, educational, and compatibility tool.

Users are responsible for ensuring that commands are executed only on systems and resources they are authorized to administer.

---

# Data and Privacy

WinCLIUtilities is designed primarily as a local command-line project.

Unless a component explicitly documents otherwise, the project does not intentionally:

* collect telemetry;
* track users;
* upload command output;
* transmit personal information;
* require a cloud account;
* require a remote service.

Users should nevertheless inspect source code and installation scripts before executing software obtained from third-party sources.

---

# Windows and Microsoft Trademark Notice

WinCLIUtilities is an independent project.

Microsoft, Windows, Windows Server, PowerShell, AccessChk, ADDUSERS, ARP, ASSOC, ATTRIB, AUDITPOL, and other Microsoft product names are trademarks or property of their respective owners.

WinCLIUtilities does not claim ownership of those names.

The names of Microsoft products and utilities are used solely for descriptive, interoperability, identification, educational, or compatibility-related purposes.

Nothing in this project should be interpreted as implying:

* endorsement;
* sponsorship;
* partnership;
* certification;
* authorization;
* affiliation.

---

# Relationship With Original Windows Utilities

WinCLIUtilities is **not a redistribution of Microsoft's original Windows executables**, unless explicitly stated otherwise for a specific component.

The project provides independent Linux implementations, wrappers, translations, or approximations.

For example:

```text
Microsoft AccessChk
        |
        v
WinCLIUtilities
        |
        +-- stat
        +-- getfacl
        +-- getcap
        +-- find
        +-- ps
        +-- systemctl
```

Similarly:

```text
Microsoft ARP
        |
        v
WinCLIUtilities ARP
        |
        v
Linux ip neigh
```

And:

```text
Microsoft AUDITPOL
        |
        v
WinCLIUtilities AUDITPOL
        |
        +-- auditctl
        +-- auditd
        +-- ausearch
        +-- aureport
        +-- augenrules
```

The implementation is therefore independent of the original Windows executables.

---

# Compatibility Disclaimer

WinCLIUtilities should not be assumed to provide:

* binary compatibility;
* API compatibility;
* ABI compatibility;
* identical scripting compatibility;
* identical output;
* identical security behavior;
* identical administrative behavior.

A script written for the original Windows utility may require modifications.

Differences can occur because of:

* operating-system architecture;
* filesystem semantics;
* permission models;
* security models;
* networking architecture;
* process management;
* service management;
* user/account management;
* file-association mechanisms;
* audit architectures.

The project aims for:

> **Practical familiarity and interoperability, not perfect emulation.**

---

# Accuracy Disclaimer

Reasonable efforts may be made to reproduce the terminology, workflow, and behavior of corresponding Windows utilities.

However, WinCLIUtilities may produce results that differ from the original Windows implementation.

No guarantee is made that:

* output is identical;
* every command-line option behaves identically;
* every Windows feature is supported;
* every Windows security concept has a Linux equivalent;
* every Linux distribution behaves identically;
* every filesystem supports every operation;
* every dependency behaves identically across versions.

Users should verify system state after performing administrative or security-sensitive operations.

---

# Administrative and Security Use

WinCLIUtilities can interact with security-sensitive parts of a Linux system.

Examples include:

```text
user accounts
groups
permissions
ACLs
filesystem attributes
network configuration
services
audit rules
system configuration
```

Incorrect use can potentially result in:

* inaccessible files;
* changed permissions;
* locked accounts;
* deleted accounts;
* altered network configuration;
* changed audit policies;
* reduced system visibility;
* service failures;
* unexpected system behavior.

Users should understand the operation being performed before applying it to production systems.

For critical systems, testing in a controlled environment is strongly recommended.

---

# Backup Recommendation

Before performing administrative changes, users should maintain appropriate backups.

Particular care should be taken when modifying system configuration or security-related files such as:

```text
/etc/passwd
/etc/group
/etc/shadow
/etc/audit/
/etc/audit/rules.d/
/etc/mime.types
~/.config/mimeapps.list
```

The exact files involved depend on:

* Linux distribution;
* installed packages;
* desktop environment;
* system configuration;
* specific WinCLIUtilities command.

---

# Filesystem Compatibility

Some functionality depends on filesystem capabilities.

For example:

```bash
chattr
lsattr
```

support filesystem-specific attributes.

An operation that works on:

```text
ext4
```

may behave differently or be unavailable on:

```text
NTFS
exFAT
FAT32
network filesystems
virtual filesystems
container filesystems
```

Users should consult the documentation for their filesystem before relying on filesystem-specific behavior.

---

# AUDITPOL Security Considerations

Linux Audit is a powerful security subsystem.

Poorly designed audit rules may generate significant amounts of data.

Potential consequences include:

* excessive logging;
* increased disk usage;
* increased CPU usage;
* increased I/O;
* difficult-to-manage audit logs;
* excessive numbers of irrelevant events.

Users should avoid blindly applying large audit-rule collections to production systems.

Audit configurations should be tested and reviewed before deployment.

---

# Responsible Use

Users are responsible for ensuring that they have authorization to operate on systems where WinCLIUtilities is used.

The project should not be used to:

* access systems without authorization;
* bypass authentication;
* circumvent security controls;
* interfere with systems belonging to others;
* modify accounts without authorization;
* disable security monitoring without authorization;
* evade auditing or logging;
* perform unauthorized network operations.

The existence of a command or option does not imply that its use is authorized in every environment.

---

# Open Source

WinCLIUtilities is intended to be developed as an open-source project.

Contributions are welcome, including:

* bug fixes;
* compatibility improvements;
* documentation;
* tests;
* additional utilities;
* distribution-specific improvements;
* security improvements;
* output-format improvements;
* performance improvements.

Contributors should avoid introducing functionality designed to bypass operating-system security controls or perform unauthorized actions.

---

# Contributing

Before contributing, contributors should:

1. Read the project documentation.
2. Understand the Linux subsystem involved.
3. Test changes locally.
4. Document compatibility limitations.
5. Avoid unnecessary dependencies.
6. Avoid hard-coded credentials.
7. Avoid collecting unnecessary data.
8. Avoid introducing undisclosed network communication.
9. Avoid security-control bypasses.
10. Update documentation when command behavior changes.

---

# Pull Requests

Before submitting a pull request:

1. Test the affected utility.
2. Verify behavior on the intended Linux distribution.
3. Document distribution-specific dependencies.
4. Verify error handling.
5. Avoid unnecessary privileged operations.
6. Do not introduce hard-coded credentials or secrets.
7. Do not introduce undocumented telemetry.
8. Do not introduce code intended to bypass security mechanisms.
9. Update the README or relevant documentation.
10. Include a clear explanation of behavioral differences from the original Windows utility.

---

# Issue Reporting

When reporting a bug, provide:

* Linux distribution;
* distribution version;
* Bash version;
* kernel version where relevant;
* filesystem involved;
* command executed;
* arguments used;
* expected behavior;
* actual behavior;
* relevant error output.

Do not publish sensitive information such as:

* passwords;
* private keys;
* authentication tokens;
* personal information;
* private network information;
* confidential configuration;
* secrets contained in command output.

Redact sensitive information before submitting an issue.

---

# Third-Party Software

WinCLIUtilities may rely on software provided by the underlying Linux distribution.

Examples include:

```text
bash
coreutils
util-linux
acl
attr
iproute2
auditd
systemd
xdg-utils
```

These projects are independent from WinCLIUtilities.

They remain subject to their respective licenses, copyrights, trademarks, and terms.

WinCLIUtilities does not claim ownership of third-party software.

---

# Licensing

The WinCLIUtilities source code should be distributed under the open-source license selected for the project.

A `LICENSE` file should be included in the repository and should contain the complete text of the selected license.

Possible licenses include, for example:

* MIT;
* Apache License 2.0;
* GNU GPL;
* BSD licenses.

The final project should select **one clearly defined license** rather than relying on an informal licensing statement.

Third-party software and dependencies remain subject to their respective licenses.

Third-party trademarks remain the property of their respective owners.

---

# Warranty Disclaimer

WinCLIUtilities is provided on an **"AS IS"** and **"AS AVAILABLE"** basis, to the maximum extent permitted by applicable law.

To the maximum extent permitted by applicable law, the authors and contributors disclaim warranties of any kind, whether express, implied, statutory, or otherwise, including warranties relating to:

* merchantability;
* fitness for a particular purpose;
* non-infringement;
* reliability;
* availability;
* accuracy;
* compatibility;
* suitability for a particular environment.

No guarantee is made that the software will:

* work on every Linux distribution;
* work with every filesystem;
* reproduce Windows behavior exactly;
* operate without errors;
* remain compatible with every future dependency;
* satisfy a particular administrative requirement;
* satisfy a particular security requirement.

Nothing in this section is intended to exclude or limit any legal rights or liability that cannot lawfully be excluded or limited.

---

# Limitation of Liability

To the maximum extent permitted by applicable law, the authors and contributors shall not be liable for damages or losses arising from the use of, inability to use, modification of, or reliance upon WinCLIUtilities.

Potential losses may include, where legally permitted:

* data loss;
* filesystem damage;
* configuration loss;
* account lockouts;
* service interruption;
* network disruption;
* security incidents;
* system downtime;
* corrupted files;
* audit configuration changes;
* loss of business;
* loss of productivity.

Nothing in this section is intended to exclude or limit liability where such exclusion or limitation is prohibited by applicable law.

---

# Project Status

WinCLIUtilities is an evolving project.

Individual utilities may provide:

* complete Linux-native implementations;
* partial implementations;
* compatibility wrappers;
* approximations;
* experimental functionality.

A command being available does not necessarily mean that it provides complete compatibility with the corresponding Windows utility.

Each utility should document its own limitations.

---

# Design Philosophy

The core philosophy of WinCLIUtilities is:

> **Familiar commands. Native Linux mechanisms. Honest compatibility.**

The project does not attempt to hide the differences between Windows and Linux.

Instead, it provides a familiar interface while making use of the operating system that is actually running underneath.

This makes WinCLIUtilities useful not only as a compatibility project, but also as an educational bridge between the two operating systems.

---

# Roadmap

Possible future utilities include Windows-style interfaces for:

* process management;
* service management;
* networking;
* file permissions;
* environment variables;
* system information;
* scheduled tasks;
* disk management;
* event logging;
* account management;
* system configuration;
* diagnostic tools;
* storage management;
* system monitoring.

Future implementations will prioritize:

1. Linux-native functionality;
2. transparent behavior;
3. predictable output;
4. security;
5. minimal dependencies;
6. documentation;
7. cross-distribution compatibility;
8. maintainability.

---

# Final Notice

WinCLIUtilities is an independent open-source project created to provide Windows-style command-line workflows on Linux.

It is:

* **not Microsoft Windows**;
* **not a Microsoft product**;
* **not affiliated with Microsoft**;
* **not endorsed by Microsoft**;
* **not a redistribution of Microsoft's Windows utilities** unless explicitly stated otherwise.

The project provides independent Linux implementations, wrappers, translations, or approximations intended to make certain Windows-style workflows familiar to Linux users.

Users should understand the underlying Linux operation before executing commands that modify:

* system configuration;
* permissions;
* accounts;
* networking;
* services;
* filesystems;
* security auditing.

By using WinCLIUtilities, users remain responsible for their actions and for maintaining appropriate:

* backups;
* authorization;
* security controls;
* system configuration;
* operational safeguards.

Nothing in this README is intended to provide legal advice or to exclude rights or liabilities that cannot legally be excluded.

---

# WinCLIUtilities

**Windows-style CLI utilities for Linux.**

**Non-profit. Open source. Independent.**

> Familiar commands. Native Linux mechanisms. Honest compatibility.

```
```
