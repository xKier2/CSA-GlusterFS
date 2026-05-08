# (BSCS-401) CSA-GlusterFS Web Cluster Demo
This project demonstrates a 2-node replicated storage cluster using GlusterFS and Apache2. It ensures that web content is mirrored in real-time and remains accessible even if one server fails.

## 📺 Project Demonstration
Watch the full process of the GlusterFS and RAID 1 configuration, replication test, and failover here:

### GlusterFS Web Cluster Demo
[**GlusterFS Web Cluster Demo**](https://drive.google.com/file/d/1kXJeRON5wywVMumdk-8UR6JP-rviVDX6/view?usp=sharing)

[![GlusterFS Demo](https://img.shields.io/badge/GlusterFS-Demo-8A2BE2?style=for-the-badge&logo=icloud&logoColor=white)](https://drive.google.com/file/d/1kXJeRON5wywVMumdk-8UR6JP-rviVDX6/view?usp=sharing)


#### Credits: Nathaniel Victoria (RAID 1)
[**Raid Demonstration**](https://drive.google.com/file/d/1rDcnvpCgui9_NKbqKj_mz5J98x1-IEVe/view?usp=sharing) 

[![RAID Demo](https://img.shields.io/badge/Ubuntu-RAID_1_Demo-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://drive.google.com/file/d/1rDcnvpCgui9_NKbqKj_mz5J98x1-IEVe/view?usp=sharing)

## Pre-requisite
**Network:** Ensure /etc/hosts contains the correct IPs for server1 and server2.

**Volume Name:** gv0

**Replica Count:** 2

## Commands
### Mount Recovery
```bash
# Force unmount the directory
sudo umount -l /var/www/html

# Restart the GlusterFS service
sudo systemctl restart glusterd

# Remount the volume defined in /etc/fstab
sudo mount -a
```

### Cleaning State
To ensure a clean demo environment, it is advised to wipe existing data. 
Ensure you have no important configurations in /var/www/html before proceeding.
```bash
# Run on Server 1 only (Replication will delete it from Server 2)
sudo rm -rf /var/www/html/*
```

### Adding .html file
```bash
# Create the empty index file
sudo touch /var/www/html/index.html

# Open the file to add your HTML content
sudo nano /var/www/html/index.html
# Example: <h1> Hello from BSCS401 - GlusterFS </h1>

# Set ownership to Apache so the web server can read it
sudo chown -R www-data:www-data /var/www/html
sudo chmod 644 /var/www/html/index.html

# Verify the file synchronized to the second node
# Run on Server 2:
ls -l /var/www/html/
```

### Healing Procedure
**NOTE:** Power on the one you shutdown (e.g. `Server 1`) and re-establish the connection
```bash
# Run on Server 1 once booted
sudo mount -a

# Check for the file created while the server was offline
ls /var/www/html/
```

## 👥 Contributors

* **Kier Gabriel Tongol**
* **Nathaniel Victoria**
