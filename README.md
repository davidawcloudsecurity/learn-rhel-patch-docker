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
