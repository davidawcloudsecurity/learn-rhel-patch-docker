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
To zip
```bash
tar -czvf local-repo-backup.tar.gz local-repo/
```
To untar
```bash
tar -xvzf filename.tar.gz
```
To run RHEL with a persistent volume using Docker, you can follow these steps:

### **1. Pull the Official RHEL Image**
Use the Red Hat Universal Base Image (UBI):
```bash
docker pull registry.access.redhat.com/ubi9/ubi  # RHEL 9
```

### **2. Create a Persistent Volume**
Create a Docker volume to persist data:
```bash
docker volume create rhel-data
```

### **3. Run the RHEL Container with Persistent Volume**
Mount the volume to the container using the `-v` flag:
```bash
docker run -it --rm -v rhel-data:/mnt/data registry.access.redhat.com/ubi9/ubi /bin/bash
```

### **4. Verify the Persistent Volume**
Inside the container, you can write data to the mounted directory to ensure that it persists:
```bash
echo "Persistent Data" > /mnt/data/test.txt
```

### **5. Reuse the Volume**
Run another container and mount the same volume to verify the data persists:
```bash
docker run -it --rm -v rhel-data:/mnt/data registry.access.redhat.com/ubi9/ubi /bin/bash
cat /mnt/data/test.txt
```

This ensures that your data remains intact across container lifecycles.

```bash
sudo rpm -ivh package_name.rpm  # For new installations
sudo rpm -Uvh package_name.rpm  # For upgrades
```
To install these RPM packages on a Red Hat Enterprise Linux 9 (RHEL 9) or compatible system (e.g., CentOS Stream 9, Rocky Linux 9), follow these methods:

---

## **Installation Methods**
### **1. Basic RPM Installation**
Use the `rpm` command to install individual packages. Navigate to the directory containing the RPM files and run:
```bash
sudo rpm -ivh package_name.rpm  # For new installations
sudo rpm -Uvh package_name.rpm  # For upgrades
```
**Example**:
```bash
sudo rpm -ivh bzip2-1.0.8-10.el9_5.x86_64.rpm
```
**Limitations**:  
- **Does not resolve dependencies automatically** (you must manually install dependencies first)[1][3].  
- Use `rpm -qpR package_name.rpm` to list dependencies[1].

---

### **2. YUM/DNF Installation (Recommended)**
YUM/DNF automatically resolves dependencies using system repositories. For RHEL 9, **DNF is the default**:
```bash
sudo dnf localinstall package_name.rpm
```
**Example**:
```bash
sudo dnf localinstall bzip2-devel-1.0.8-10.el9_5.x86_64.rpm
```
**Advantages**:  
- **Resolves dependencies** if packages are available in enabled repositories[1][6].  
- Works for all listed packages (e.g., `glibc`, `libgcc`, `pkgconf`)[1][6].

---

## **Key Considerations**
### **Package Compatibility**
- **Architecture**: Ensure compatibility between `.i686` (32-bit) and `.x86_64` (64-bit) packages. Mixed architectures are supported but require both versions of dependencies (e.g., `glibc.i686` and `glibc.x86_64`)[1][6].  
- **CPE Compatibility**: All listed packages (e.g., `bzip2`, `glibc`, `pkgconf`) are explicitly tagged for RHEL 9 (`cpe:/o:redhat:enterprise_linux:9`), confirming compatibility[^query].

---

### **Dependency Handling**
1. **Check Dependencies**:
   ```bash
   rpm -qpR glibc-2.34-125.el9_5.3.i686.rpm
   ```
2. **Install Missing Dependencies**:
   ```bash
   sudo dnf install dependency_name
   ```
   If dependencies are unavailable in repositories, manually install their RPMs first[1][6].

---

### **Verification**
- **Confirm Installation**:
  ```bash
  rpm -qa | grep bzip2  # Lists installed bzip2 packages
  ```
- **Validate Signatures** (optional):
  ```bash
  rpm -Kv package_name.rpm  # Checks cryptographic integrity[4][6]
  ```

---

## **Troubleshooting**
- **"Already Installed" Errors**: Use `rpm -Uvh` instead of `-ivh` for upgrades[3][5].  
- **Missing Dependencies**: Enable additional repositories (e.g., EPEL) or download missing RPMs manually[1][6].  
- **Conflicts**: Remove conflicting packages with `sudo dnf remove package_name` before reinstalling[6]. 

For GUI-based installations, double-click the RPM file in GNOME/KDE file managers, but this method is less reliable for dependency-heavy packages[1].

Citations:
[1] https://phoenixnap.com/kb/how-to-install-rpm-file-centos-linux
[2] https://servicenow.iu.edu/kb?id=kb_article_view&sysparm_article=KB0025243
[3] https://access.redhat.com/solutions/1189
[4] https://www.ibm.com/docs/en/sdk-java-technology/8?topic=installing-rpm-packages-linux-only
[5] https://www.geeksforgeeks.org/how-to-install-rpm-packages-on-linux/
[6] https://www.dedicatedcore.com/blog/install-rpm-file-linux-os-centos-fedora/
[7] https://askubuntu.com/questions/2988/how-do-i-install-and-manage-rpms
[8] https://unix.stackexchange.com/questions/43114/how-to-install-remove-upgrade-rpm-packages-on-red-hat

---
Answer from Perplexity: https://www.perplexity.ai/search/ho-wto-install-these-rpm-in-os-VT.Cwi0rRmCaQPB.yANB0Q?utm_source=copy_output
