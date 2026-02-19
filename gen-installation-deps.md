# Offline Dependency Bundle (Ubuntu 24.04.3)

## How the Offline Dependencies Were Generated

The dependency bundle was generated from a clean **Ubuntu Server 24.04.3** environment prepared specifically for offline deployment testing.

Environment characteristics:

- Fresh installation of Ubuntu Server 24.04.3
- No system updates applied
- `/etc/apt/sources.list` cleared to prevent updates
- No additional repositories configured
- Dependencies downloaded using APT download-only mode

Process used to generate the dependency bundle:


```bash
mkdir offline-deps  
cd offline-deps  

apt-get update  
apt-get install --download-only \
debconf \
adduser \
procps \
apt-transport-https \
gnupg \
debhelper \
libcap2-bin
```


After dependency resolution, all required `.deb` packages were collected and bundled for offline use.

---

# Offline Dependency Package Contents

The bundle contains approximately **75 packages** required to support the Wazuh offline assisted installation and its dependency chain.

These packages fall into several groups.

---

## Core Packages Required by the Wazuh Installer

- debconf  
- adduser  
- procps  
- apt-transport-https  
- gnupg  
- libcap2-bin  
- debhelper  

These provide:

- Package configuration handling  
- User and service management  
- Repository key handling  
- Linux capability management  
- Debian packaging helper utilities  

---

## Debian Packaging Framework (Required by debhelper)

- debhelper  
- dh-autoreconf  
- dh-strip-nondeterminism  
- dpkg-dev  
- libdebhelper-perl  
- libdpkg-perl  

These packages provide packaging helpers used during installation routines.

---

## Compiler and Toolchain Dependencies

Installed as part of dependency resolution:

- build-essential  
- gcc  
- gcc-13  
- gcc-13-base  
- g++  
- g++-13  
- cpp  
- cpp-13  
- binutils  
- binutils-common  
- binutils-x86-64-linux-gnu  

---

## Runtime Libraries

- libasan8  
- libatomic1  
- libcc1-0  
- libbinutils  
- libctf0  
- libctf-nobfd0  

These libraries are dependencies required by the toolchain packages.

---

## Perl Modules Used by Debian Packaging Tools

- libarchive-zip-perl  
- libarchive-cpio-perl  
- libalgorithm-diff-perl  
- libalgorithm-diff-xs-perl  
- libalgorithm-merge-perl  
- libfile-stripnondeterminism-perl  
- libfile-fcntllock-perl  

These modules support archive processing and Debian packaging scripts.

---

## Supporting Utilities

- autoconf  
- automake  
- autotools-dev  
- gettext  
- intltool-debian  
- fakeroot  
- bzip2  
- debugedit  

---

# Purpose of This Bundle

This dependency bundle ensures the **Wazuh assisted installer runs correctly in a fully offline environment** without requiring:

- Internet connectivity  
- Ubuntu repositories  
- Additional dependency downloads  

Intended use cases:

- Air-gapped environments  
- SOC lab deployments  
- Security testing environments  
- Isolated infrastructure installations
