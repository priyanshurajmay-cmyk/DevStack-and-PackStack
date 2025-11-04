# DevStack-and-PackStack
# DevStack vs Packstack

## Overview

| Feature | **DevStack** | **Packstack** |
|--------|--------------|--------------|
| Purpose | Development & testing environment for OpenStack | Simplified deployment method for OpenStack (PoC / lab use) |
| Target Audience | Developers, contributors | System admins, testers |
| Installation Source | Builds from source | Installs from RPM packages |
| Deployment Type | Temporary, disposable | Semi-persistent (lab / demo) |
| Production Use |  Not recommended |  Not recommended, but closer |
| Configuration | Lightweight scripts | Puppet automation |
| Speed | Faster to deploy & reset | Slower initial setup |
| Flexibility | Very flexible & hackable | Limited customization |
| Typical Use Case | Testing OpenStack features, development environments | Quick OpenStack PoC or training lab on RHEL/CentOS |

---

## When to Use What

### Use **DevStack** if:
- You're learning internals
- You want to modify OpenStack services
- You need a quick, temporary environment
- You're doing development or bug investigation

### Use **Packstack** if:
- You want a more stable lab setup
- You're doing admin training
- You need something closer to a real deployment
- You're working on RHEL/CentOS environment

---


**DevStack installation guide** (Ubuntu-based)

---

## ✅ **DevStack Installation Guide (Ubuntu 22.04 / 20.04)**

### **1. System Requirements**

| Requirement | Value                              |
| ----------- | ---------------------------------- |
| OS          | Ubuntu 22.04 / 20.04 (recommended) |
| RAM         | 8 GB minimum (16 GB preferred)     |
| CPU         | 4 cores minimum                    |
| Disk        | 30+ GB free                        |
| User        | Non-root **sudo** user             |
| Networking  | Internet access required           |

---

### **2. Prepare System**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install git -y
sudo apt install python3-dev python3-pip -y
```

Create a non-root `stack` user (DevStack requires it):

```bash
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
```

Switch to stack user:

```bash
sudo su - stack
```

---

### **3. Clone DevStack**

```bash
git clone https://opendev.org/openstack/devstack
cd devstack
```

---

### **4. Create `local.conf`**

```bash
nano local.conf
```

Paste:

```ini
[[local|localrc]]
ADMIN_PASSWORD=admin
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP=127.0.0.1

# Recommended services
enable_service horizon
enable_service glance
enable_service keystone
enable_service neutron
enable_service nova
enable_service placement-api
```

Save & exit.

---

### **5. Start Installation**

```bash
./stack.sh
```



---

### **6. Access OpenStack Dashboard**

After successful install:

Open browser:

```
http://<your-ip>/dashboard
```

Username:

```
admin
```

Password:

```
admin
```

---

<img width="1919" height="1033" alt="image" src="https://github.com/user-attachments/assets/bfc073dd-af37-4d4f-974a-66e021551761" />


### **7. Manage DevStack**

Stop DevStack:

```bash
./unstack.sh
```

Clean everything:

```bash
./clean.sh
```

---

### **Troubleshooting Tips**

| Issue                | Fix                                    |
| -------------------- | -------------------------------------- |
| Memory errors        | Increase RAM / swap                    |
| Network DHCP issues  | Restart `neutron` or reboot            |
| Keystone auth errors | Ensure passwords match in `local.conf` |
| Re-install stuck     | Run `./clean.sh` then `./stack.sh`     |

---

### **VM Notes**

If installing inside VirtualBox / VMware:

* Enable **VT-x / AMD-V**
* Set **bridged networking**
* Allocate **≥8GB RAM, ≥4 vCPU**

---


