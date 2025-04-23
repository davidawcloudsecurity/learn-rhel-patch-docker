# learn-rhel-patch-docker
how to patch rhel9 with docker

## **Core Methods**
**1. Pull Official RHEL Image**  
Use Red Hat's Universal Base Image (UBI) for containerized RHEL:
```bash
docker pull registry.access.redhat.com/ubi8/ubi  # RHEL 8
docker pull registry.access.redhat.com/ubi9/ubi  # RHEL 9
```

**2. Run Interactive Container**  
For temporary shell access:
```bash
docker run -it --rm registry.access.redhat.com/ubi8/ubi /bin/bash
```
**3. Initiate** 
```bash
yum install yum-utils;
```
**4. Register** 
```bash
subscription-manager register --username <username> --password <password>
```
**Use reposync command to download the necessary updates**
reposync --repoid=rhel-9-for-x86_64-baseos-rpms --download-path=/tmp/local-repo/
**Specfic rpm**
```bash
yumdownloader --resolve grub2-efi-x64* --destdir=./x64-pkgs
```
