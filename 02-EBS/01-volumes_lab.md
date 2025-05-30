
---

## **Lab Exercise 1: Basic EBS Volume Attachment to an EC2 Instance**  
**Objective**: Attach an EBS volume to a running EC2 instance, format it, and mount it for use.  

### **Steps**  
1. **Launch an EC2 Instance** (Amazon Linux 2, `t2.micro`).  
2. **Create an EBS Volume**:  
   - Go to **EC2 → Volumes → Create Volume**.  
   - Select **GP3 (General Purpose SSD)**, **10 GiB**, same AZ as your instance.  
3. **Attach the Volume to the Instance**:  
   - Select the volume → **Actions → Attach Volume**.  
   - Choose your instance and set the device name (e.g., `/dev/sdf`).  
4. **SSH into the Instance and Configure the Volume**:  
   ```bash
   # List available disks (find the new volume, e.g., /dev/xvdf)
   lsblk

   # Format the volume as ext4
   sudo mkfs -t ext4 /dev/xvdf

   # Create a mount directory and mount the volume
   sudo mkdir /mnt/data
   sudo mount /dev/xvdf /mnt/data

   # Verify
   df -h
   ```  
5. **Make the Mount Persistent**:  
   ```bash
   sudo bash -c 'echo "/dev/xvdf /mnt/data ext4 defaults 0 0" >> /etc/fstab'
   ```  
6. **Test**:  
   - Create a file (`sudo touch /mnt/data/test.txt`) and reboot to verify persistence.  

**Use Case**: Adding extra storage to a server for logs, apps, or databases.  

---

## **Lab Exercise 2: Expanding an EBS Volume (Disk Resizing)**  
**Objective**: Increase the size of an attached EBS volume and extend the filesystem without downtime.  

### **Steps**  
1. **Start with Lab 1’s setup** (attached and mounted 10 GiB volume).  
2. **Modify the EBS Volume Size**:  
   - Go to **EC2 → Volumes → Select Volume → Modify Volume**.  
   - Increase size to **20 GiB** and confirm.  
3. **Extend the Filesystem**:  
   ```bash
   # Check current size (should still show 10G)
   df -h

   # Expand the partition (for xfs/ext4)
   sudo growpart /dev/xvdf 1  # Adjust partition number if needed
   sudo resize2fs /dev/xvdf1  # (For ext4)
   # OR for XFS: sudo xfs_growfs /mnt/data

   # Verify new size
   df -h
   ```  
4. **Test**:  
   - Write a large file (`dd if=/dev/zero of=/mnt/data/largefile bs=1M count=5000`) to confirm space.  

**Use Case**: Scaling storage for a growing database or application.  

---

## **Lab Exercise 3: EBS Snapshots & Restoring to a New Volume**  
**Objective**: Create a snapshot of an EBS volume and restore it to a new volume.  

### **Steps**  
1. **Use Lab 1’s setup** (volume mounted at `/mnt/data`).  
2. **Create a Snapshot**:  
   - Go to **EC2 → Snapshots → Create Snapshot**.  
   - Select the volume, add a description (`My-Data-Backup`).  
3. **Simulate Data Loss**:  
   ```bash
   sudo rm -rf /mnt/data/*
   echo "Data deleted!" > /mnt/data/deleted.txt
   ```  
4. **Restore the Snapshot to a New Volume**:  
   - Select the snapshot → **Actions → Create Volume**.  
   - Choose same AZ, GP3, 10 GiB.  
5. **Replace the Old Volume**:  
   - Detach the old volume (**Actions → Detach Volume**).  
   - Attach the new volume to the instance as `/dev/xvdf`.  
6. **Remount and Verify**:  
   ```bash
   sudo mount /dev/xvdf /mnt/data
   ls /mnt/data  # Original files should reappear!
   ```  

**Use Case**: Disaster recovery or cloning volumes across AZs/Regions.  

---
