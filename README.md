# WinCLIUtilities

**Windows-style CLI utilities for Linux.**

WinCLIUtilities is a non-profit open-source project that provides Linux-compatible command-line utilities inspired by the behavior, syntax, workflows, and concepts of various utilities traditionally available on Microsoft Windows.

The project is designed for Linux users who are familiar with Windows command-line tools and want comparable workflows using native Linux mechanisms.

> **NON-PROFIT PROJECT**
>
> WinCLIUtilities is developed and distributed for educational, interoperability, experimentation, administration, and personal use. It is not affiliated with, endorsed by, sponsored by, or officially connected to Microsoft Corporation.

---

## Overview

Linux and Windows provide many utilities that solve similar administrative and diagnostic problems, but their command-line interfaces, permission systems, filesystem models, networking stacks, and security architectures are substantially different.

WinCLIUtilities attempts to bridge this gap by providing Windows-style command interfaces on Linux while relying, whenever possible, on the underlying Linux-native tools and security mechanisms.

Examples include:

- `ACCESSCHK`
- `ADDUSERS`
- `ARP`
- `ASSOC`
- `ATTRIB`
- `AUDITPOL`

The goal is **not** to recreate Windows internally.

Instead, WinCLIUtilities provides a familiar command-line interface that translates or approximates Windows-oriented workflows into Linux-native operations.

---

# Project Goals

WinCLIUtilities aims to:

- provide familiar Windows-style commands on Linux;
- help Windows users transition to Linux;
- provide compatibility-oriented CLI workflows;
- expose Linux functionality through recognizable Windows-style syntax;
- demonstrate differences between Windows and Linux administration;
- provide educational examples of shell scripting and system administration;
- simplify administration for users familiar with Windows utilities;
- remain lightweight and transparent;
- use existing Linux utilities whenever possible instead of reinventing system functionality.

---

# Important: Windows Compatibility vs. Behavioral Equivalence

WinCLIUtilities does **not** claim that Linux and Windows are technically equivalent.

Many Windows utilities operate on concepts that have no direct Linux equivalent.

For example:

| Windows concept | Linux equivalent |
|---|---|
| NTFS ACL | POSIX ACL / Linux ACL |
| Windows Registry | No universal equivalent |
| Windows file attributes | Linux permissions / extended attributes / filesystem flags |
| Windows Event Log | systemd journal / auditd / other logging systems |
| Windows Security Auditing | Linux Audit (`auditd`) |
| Windows services | systemd services |
| Windows file associations | MIME types / `.desktop` files |
| Windows networking stack | Linux networking stack |
| Windows user privileges | Unix permissions, groups, capabilities, sudo |
| Windows SIDs | Unix UID/GID |
| Windows security descriptors | Linux permissions/ACLs/capabilities |

Consequently, some WinCLIUtilities commands provide:

### Native equivalents

The Linux subsystem already provides a functionally similar mechanism.

### Translations

The Windows-style command is translated into an appropriate Linux operation.

### Approximations

The Windows concept does not exist directly on Linux, so the closest practical Linux mechanism is used.

### Informational compatibility

The command provides information in a Windows-like format without pretending that the underlying Linux security model is identical.

This distinction is intentional.

---

# Current Utilities

## ACCESSCHK

Windows-style interface for inspecting Linux permissions, ACLs, capabilities, processes, services, accounts, and network information.

Examples:

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

Linux mechanisms may include:

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

The output is designed to be understandable to users familiar with Windows AccessChk.

ADDUSERS

Windows-style account-management interface for creating and managing Linux users and groups.

The Linux implementation relies on native account-management tools such as:

useradd
usermod
userdel
groupadd
groupdel
chage
passwd

Example:

addusers
addusers users.csv
addusers -d users.csv

The CSV format used by the project may contain fields such as:

Username,Full Name,Password,Description,Home,Shell,Groups
Important difference

Windows and Linux account systems are different.

Options such as password expiration, account locking, password changes, group membership, and account privileges are therefore implemented using Linux's own account-management mechanisms.

The command should not be considered a drop-in replacement for Microsoft's original ADDUSERS.exe.

ARP

Windows-style interface for the Linux neighbor table.

Example:

arp
arp -a
arp -g
arp -v
arp -d 192.168.1.10
arp -d *
arp -s 192.168.1.10 00-11-22-33-44-55

The implementation uses Linux networking facilities such as:

ip neigh

MAC addresses may therefore be displayed using Linux conventions.

Administrative privileges may be required for operations that modify the neighbor table.

ASSOC

Windows-style file-association interface using the Linux MIME/application system.

Examples:

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

Linux associations are generally based on:

MIME types;
.desktop application files;
mimeapps.list;
XDG standards.

Therefore:

Windows:
.extension -> file type -> application

becomes approximately:

Linux:
.extension -> MIME type -> .desktop application

The two systems are not identical.

Removing an association may therefore mean removing a user-level default rather than deleting the MIME definition itself.

ATTRIB

Windows-style interface for inspecting and modifying file attributes.

Example:

attrib file.txt
attrib +R file.txt
attrib -R file.txt
attrib /S /D /R *.txt

Linux does not implement Windows file attributes in the same way.

Depending on the operation, WinCLIUtilities may use:

Unix permission bits;
filesystem attributes;
chattr;
lsattr;
extended attributes;
filename conventions.

For example, Linux provides filesystem flags such as:

i   immutable
a   append-only
A   no atime updates
c   compressed
d   no dump

These are not identical to Windows attributes.

In particular, a Linux hidden file is normally identified by its filename beginning with .:

.config
.bashrc
.local

Linux does not have a universal Windows-style H attribute.

AUDITPOL

Windows-style auditing interface backed by Linux Audit.

The Linux implementation is based on:

auditd
auditctl
ausearch
aureport
augenrules

Example:

auditpol /?
auditpol /get
auditpol /list /category
auditpol /list /subcategory
auditpol /backup /file:audit-backup.rules
auditpol /restore /file:audit-backup.rules
auditpol /clear /y

Linux Audit rules are fundamentally different from Windows audit policies.

For example, Linux can monitor:

file access
file modifications
command execution
authentication events
identity changes
privilege use
network-related activity
kernel events
mount operations
system configuration changes

A Linux audit rule may look conceptually like:

-w /etc/passwd -p wa -k identity

This means that Linux Audit is being used to watch a specific resource rather than reproducing the Windows Security Auditing architecture.

Security note

Audit configuration can have significant performance and security implications.

Some operations require root privileges.

WinCLIUtilities does not guarantee that a Linux audit configuration provides the same coverage as a Windows audit policy.

Installation
Requirements

WinCLIUtilities is primarily intended for Linux systems using:

Bash
standard GNU/Linux utilities
XDG utilities where applicable
Linux Audit where applicable
POSIX permissions and ACLs

Recommended packages on Debian/Ubuntu/Kubuntu systems include:

sudo apt install acl attr xdg-utils auditd audispd-plugins

Some functionality may additionally depend on utilities already provided by the distribution.

For filesystem attributes, chattr and lsattr are normally provided by the system's filesystem utilities.

Installation From Source

Clone the repository:

git clone https://github.com/USERNAME/WinCLIUtilities.git
cd WinCLIUtilities

Then install/source the shell utilities according to the project's installation instructions.

For a user-level Bash installation, functionality may be loaded from:

~/.bashrc

After modifying .bashrc, reload it:

source ~/.bashrc

or:

. ~/.bashrc
Shell Compatibility

The project primarily targets:

Bash

It is not guaranteed to work correctly under:

sh
dash
fish
csh
other shells

unless explicitly documented.

Privileges

Some utilities require elevated privileges.

For example:

sudo auditctl

may be required for audit configuration.

Similarly, modifying:

users;
groups;
system services;
network neighbor entries;
filesystem attributes;
protected files;
system audit rules

may require root privileges.

WinCLIUtilities does not bypass Linux security controls.

If the underlying Linux command requires administrative privileges, the user must provide them through the normal Linux security mechanisms.

Security Philosophy

WinCLIUtilities is intended to operate through existing Linux security mechanisms.

The project does not attempt to:

bypass authentication;
bypass file permissions;
circumvent sudo;
disable mandatory security controls;
exploit vulnerabilities;
obtain unauthorized privileges;
circumvent system security policies.

The project is an administrative and interoperability tool.

Users are responsible for ensuring that commands are executed only on systems and resources they are authorized to administer.

Data and Privacy

WinCLIUtilities is designed to operate locally.

Unless a future component explicitly documents otherwise, the project does not intentionally:

collect telemetry;
track users;
upload command output;
transmit personal information;
require a cloud account;
require a remote service.

Users should nevertheless inspect scripts before execution, particularly when installing software obtained from third-party sources.

No Affiliation With Microsoft

WinCLIUtilities is an independent project.

The names of Microsoft products, utilities, commands, and technologies referenced in this project are used solely for:

interoperability;
identification;
documentation;
educational purposes;
describing compatibility concepts.

Microsoft, Windows, Windows Server, PowerShell, AccessChk, ADDUSERS, ARP, ASSOC, ATTRIB, AUDITPOL, and other Microsoft product names remain trademarks or property of their respective owners.

WinCLIUtilities does not claim ownership of those names.

Relationship With Original Utilities

WinCLIUtilities is not a redistribution of Microsoft's original Windows executables unless explicitly stated otherwise.

The project provides Linux shell implementations, wrappers, translations, or approximations designed to reproduce useful workflows.

For example:

Microsoft AccessChk
        ↓
Linux implementation
        ↓
getfacl / stat / getcap / find / ps / systemctl / ...

The same principle applies to the other utilities.

Compatibility Disclaimer

WinCLIUtilities should not be assumed to provide binary, API, ABI, scripting, output, or behavioral compatibility with the original Windows programs.

A script written for the original Windows utility may require modifications.

Differences can occur because of:

operating-system architecture;
filesystem semantics;
permission models;
security models;
networking architecture;
process management;
service management;
user/account management;
file-association mechanisms;
audit architectures.

The project aims for familiarity and practical interoperability, not perfect emulation.

Accuracy Disclaimer

Although reasonable efforts may be made to reproduce the behavior and terminology of the corresponding Windows utilities, WinCLIUtilities may produce results that differ from the original Windows implementation.

No guarantee is made that:

output is identical;
every command-line option behaves identically;
every Windows feature is supported;
every Windows security concept has a Linux equivalent;
every Linux distribution behaves identically;
every filesystem supports every operation;
every version of a dependency behaves identically.

Always verify the resulting system state when performing administrative or security-sensitive operations.

Administrative and Security Use

WinCLIUtilities can interact with security-sensitive parts of a Linux system.

Examples include:

user accounts
groups
permissions
ACLs
filesystem attributes
network configuration
services
audit rules
system configuration

Incorrect use can potentially result in:

inaccessible files;
modified permissions;
locked accounts;
deleted accounts;
altered network configuration;
changed audit policies;
reduced system visibility;
service failures;
unexpected system behavior.

Users should understand the command being executed before applying it to production systems.

For critical systems, test commands in a controlled environment first.

Backup Recommendation

Before making administrative changes, users should maintain appropriate backups.

This is particularly important when modifying:

/etc/passwd
/etc/group
/etc/shadow
/etc/audit/
/etc/audit/rules.d/
/etc/mime.types
~/.config/mimeapps.list

The exact files involved depend on the Linux distribution and the operation being performed.

Distribution Compatibility

WinCLIUtilities is primarily designed around standard Linux interfaces.

Behavior may differ between distributions such as:

Debian
Ubuntu
Kubuntu
Linux Mint
Fedora
Arch Linux
openSUSE
RHEL
Rocky Linux
AlmaLinux

Package names and default configuration locations may vary.

For this reason, the project should identify distribution-specific behavior where necessary.

Filesystem Compatibility

Some operations depend on the filesystem.

For example, Linux filesystem attributes such as those managed through:

chattr
lsattr

are not universally supported by every filesystem.

An operation that works on:

ext4

may behave differently or be unavailable on:

NTFS
exFAT
FAT32
network filesystems
virtual filesystems
container filesystems

Users should consult the documentation of their filesystem before relying on filesystem-specific features.

AuditPOL Security Considerations

The auditpol implementation deserves particular attention.

Linux Audit is a powerful security subsystem.

Audit rules can generate significant amounts of data.

Poorly designed rules may cause:

excessive logging;
increased disk usage;
increased CPU overhead;
difficult-to-manage audit logs;
large amounts of irrelevant events.

Therefore, avoid blindly applying large collections of audit rules to production systems.

Test audit configurations before deployment.

Root Privileges

The project intentionally respects Linux privilege boundaries.

Commands that require root should normally be executed using:

sudo

rather than attempting to bypass the system's privilege model.

Example:

sudo auditpol /set ...

if the implementation requires elevated privileges.

Error Handling

Errors returned by underlying Linux commands should be considered meaningful.

For example:

Permission denied

does not necessarily indicate a WinCLIUtilities bug.

It may mean that Linux correctly prevented the requested operation.

Likewise:

Operation not supported

may indicate that the current filesystem or subsystem does not implement the requested feature.

Intended Use

WinCLIUtilities is intended for:

Linux administration;
Windows-to-Linux migration;
system administration education;
interoperability experiments;
scripting;
automation;
testing;
laboratory environments;
personal computers;
development environments;
learning Linux security concepts.
Not Intended To Replace Official Documentation

WinCLIUtilities should not be considered a replacement for:

Linux distribution documentation;
man pages;
Microsoft documentation;
filesystem documentation;
kernel documentation;
auditd documentation;
security policies;
vendor documentation.

For security-sensitive operations, consult the documentation of the underlying Linux subsystem.

Open Source

WinCLIUtilities is intended to be developed as an open-source project.

Contributions are welcome, including:

bug fixes;
compatibility improvements;
documentation;
tests;
additional utilities;
distribution-specific fixes;
security improvements;
output-format improvements.

Contributors should avoid introducing functionality that bypasses operating-system security controls or performs unauthorized actions.

Pull Requests

Before submitting a pull request:

Test the affected command.
Verify behavior on the intended Linux distribution.
Document distribution-specific dependencies.
Avoid unnecessary privileged operations.
Do not introduce hard-coded credentials or secrets.
Do not introduce telemetry without explicit documentation.
Do not introduce code intended to bypass security mechanisms.
Update the documentation when command behavior changes.
Issue Reporting

When reporting a bug, provide:

Linux distribution;
distribution version;
Bash version;
filesystem involved;
command executed;
arguments used;
expected behavior;
actual behavior;
relevant error output.

Do not publish:

passwords;
private keys;
authentication tokens;
personal information;
private network information;
confidential system configuration.
Responsible Use

Users are responsible for ensuring that they have authorization to operate on the systems where WinCLIUtilities is used.

The project should not be used to:

access systems without authorization;
bypass authentication;
circumvent security controls;
interfere with systems belonging to others;
modify accounts without authorization;
disable security monitoring without authorization;
evade auditing or logging;
perform unauthorized network operations.

The availability of a command does not imply that its use is authorized in every environment.

Warranty Disclaimer

WinCLIUtilities is provided on an "AS IS" and "AS AVAILABLE" basis, to the extent permitted by applicable law.

To the maximum extent permitted by applicable law, the authors and contributors disclaim warranties of any kind, whether express, implied, statutory, or otherwise, including warranties relating to:

merchantability;
fitness for a particular purpose;
non-infringement;
reliability;
availability;
accuracy;
compatibility;
suitability for a particular environment.

No guarantee is made that the software will:

work on every Linux distribution;
work with every filesystem;
reproduce Windows behavior exactly;
operate without errors;
remain compatible with every future dependency;
satisfy a particular administrative or security requirement.
Limitation of Liability

To the maximum extent permitted by applicable law, the authors and contributors shall not be liable for damages or losses arising from the use of, inability to use, modification of, or reliance upon WinCLIUtilities.

This may include, where legally permitted:

data loss;
filesystem damage;
configuration loss;
account lockouts;
service interruption;
network disruption;
security incidents;
system downtime;
corrupted files;
audit configuration changes;
loss of business or productivity.

Nothing in this disclaimer is intended to exclude or limit liability where such exclusion or limitation is prohibited by applicable law.

Third-Party Software

WinCLIUtilities may rely on software provided by the underlying Linux distribution.

Examples include:

bash
coreutils
util-linux
acl
attr
iproute2
auditd
systemd
xdg-utils

These projects are separate from WinCLIUtilities and remain subject to their respective licenses and terms.

WinCLIUtilities does not claim ownership of third-party software.

Trademark Notice

All third-party trademarks belong to their respective owners.

The use of names such as:

Windows
Microsoft
AccessChk
ADDUSERS
ARP
ASSOC
ATTRIB
AUDITPOL

is descriptive and intended to communicate compatibility, inspiration, or functional relationship.

It does not imply endorsement, sponsorship, partnership, or affiliation.

Licensing

Unless otherwise stated in a file or directory, the source code of WinCLIUtilities is intended to be distributed under the project's chosen open-source license.

Third-party components, documentation, trademarks, and dependencies remain subject to their respective licenses.

Before publishing the repository, ensure that the final LICENSE file accurately reflects the license selected by the project.

Project Status

WinCLIUtilities is an evolving project.

Some utilities may provide:

complete Linux-native implementations;
partial implementations;
compatibility wrappers;
approximations;
experimental functionality.

A command marked as compatible should therefore be understood in the context of its documented Linux implementation.

Design Principle

The core philosophy of WinCLIUtilities can be summarized as:

Familiar commands. Native Linux mechanisms. Honest compatibility.

The project does not attempt to hide the differences between Windows and Linux.

Instead, it provides a familiar interface while making use of the operating system that is actually running underneath.

Example

A Windows-oriented workflow:

ATTRIB file.txt

may become:

attrib file.txt

while the Linux implementation may internally rely on:

lsattr
stat
chmod

Similarly:

ARP -A

may become:

arp -a

and internally use:

ip neigh

The user receives a familiar interface while Linux remains responsible for the actual operation.

Roadmap

Possible future utilities include additional Windows-style commands for:

process management;
service management;
networking;
file permissions;
environment variables;
system information;
scheduled tasks;
disk management;
event logging;
account management;
system configuration;
diagnostic tools.

Future implementations will prioritize:

Linux-native functionality;
transparent behavior;
predictable output;
security;
minimal dependencies;
documentation;
cross-distribution compatibility.
Final Notice

WinCLIUtilities is an independent open-source project created to provide Windows-style command-line workflows on Linux.

It is not Microsoft Windows, does not contain Microsoft's operating system, and should not be considered an official implementation of Microsoft's utilities.

Users should understand the underlying Linux operation before executing commands that modify system configuration, permissions, accounts, networking, or security auditing.

By using WinCLIUtilities, users remain responsible for their actions and for maintaining appropriate backups, authorization, and security controls on the systems they administer.
