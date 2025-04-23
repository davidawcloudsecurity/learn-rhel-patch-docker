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
