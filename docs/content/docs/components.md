---
title: "Components"
linkTitle: "Components"
weight: 3
description: |
    Core components included in Hadron Linux distribution
---

# Core Components

Hadron Linux is built from scratch with carefully selected upstream components. This page documents the main components and their installation locations.

## OpenSSH

Hadron includes OpenSSH for secure remote access and file transfer.

### Version
- **OpenSSH 10.0p1** (portable version)

### Installation Paths

SSH is installed in the following locations within the Hadron filesystem:

- **Binaries**: `/usr/bin/`
  - `sshd` - SSH daemon
  - `ssh` - SSH client
  - `ssh-keygen` - Key generation tool
  - Other SSH utilities

- **Configuration**: `/etc/ssh/`
  - `sshd_config` - Main SSH daemon configuration
  - `sshd_config.d/` - Drop-in configuration directory
  - `sshd_config.d/99-hadron.conf` - Hadron-specific SSH configuration
  - Host keys (generated at first boot)

- **Library Files**: `/usr/lib/ssh/`
  - Helper executables and libraries

- **Data Files**: `/usr/share/openssh/`
  - Shared data files

### Configuration

OpenSSH is configured with the following options:

- **PAM Support**: Enabled (`UsePAM yes`)
- **Privilege Separation**: Enabled with user `nobody` and path `/var/empty`
- **MD5 Passwords**: Supported
- **SSL Engine**: Enabled
- **Drop-in Configuration**: Supports additional configs in `/etc/ssh/sshd_config.d/*.conf`

### systemd Integration

SSH is managed by systemd with the following service files:

- **`sshd.service`**: Main SSH daemon service
  - Located at: `/usr/lib/systemd/system/sshd.service`
  - Starts SSH daemon in daemon mode (`/usr/bin/sshd -D`)
  - Auto-restart enabled

- **`sshd.socket`**: Socket-activated SSH service
  - Located at: `/usr/lib/etc/systemd/system/sshd.socket`
  - Listens on port 22
  - Alternative to always-running daemon (conflicts with sshd.service)

- **`sshkeygen.service`**: SSH host key generation
  - Located at: `/usr/lib/systemd/system/sshkeygen.service`
  - Runs once at first boot to generate host keys
  - Generates ECDSA, Ed25519, and RSA keys

### Build Configuration

OpenSSH is compiled from source with the following configure options:

```
--prefix=/usr
--sysconfdir=/etc/ssh
--libexecdir=/usr/lib/ssh
--datadir=/usr/share/openssh
--with-privsep-path=/var/empty
--with-privsep-user=nobody
--with-md5-passwords
--with-ssl-engine
--with-pam
--disable-lastlog
--disable-utmp
--disable-wtmp
--disable-utmpx
--disable-wtmpx
```

### Dependencies

OpenSSH depends on the following components:

- **OpenSSL**: For cryptographic operations
- **zlib**: For compression support
- **PAM**: For authentication
- **Shadow**: For user/password management

## Other Core Components

Hadron includes other essential components:

- **musl libc**: C standard library
- **systemd**: Init system and service manager
- **Linux Kernel**: Available in default and cloud variants
- **OpenSSL**: Cryptographic library
- **PAM**: Pluggable Authentication Modules
- **iptables/nftables**: Firewall utilities
- **rsync**: File synchronization utility
- **sudo**: Privilege elevation tool

For more details on specific components, refer to the Dockerfile in the repository.
