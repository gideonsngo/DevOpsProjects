# 102 - Web Solution With Wordpress

### Step 1: Prepare the Web Server
Launch an EC2 Instance that will serve as the "web server". Create 3 volumes in the same Availability Zone (AZ) as the web server each of 10GiB.

Attach all 3 volumes one by one to your web server EC2 instance.

Open up the Linux Terminal to begin configuration

   - **SSH into the instance**
  
    ```
    ssh -i keypair.pem ec2-user@public-ip
    ```
   - Inspect attached block devices: `lsblk`
   - Check all mounts and free space: `df -h`

- **Create Partitions on Each Disk**:
   - Use `gdisk` to create a single partition on each of the three disks:
     ```sh
     sudo gdisk /dev/xvdf
     sudo gdisk /dev/xvdg
     sudo gdisk /dev/xvdh
     ```
     ![Partition Vol](./images/creating-partition-with-disk.PNG)

    - Inspect attached block devices: `lsblk`

        ![Verify Partition](./images/verify-partition-disk.PNG)
    

- **Install and Configure LVM**:
   - Install LVM: `sudo yum install lvm2`
   - Check available partitions: `sudo lvmdiskscan`
    
        ![LVM Disk Scan](./images/run-lvmdiskscan-command-show-partition.PNG)

   - Mark disks as physical volumes:
     ```sh
     sudo pvcreate /dev/xvdf1
     sudo pvcreate /dev/xvdg1
     sudo pvcreate /dev/xvdh1
     ```
     ![Physical Vol](./images/creating-physical-volume.PNG)

- **Create a Volume Group**:
   - Create a volume group named `webdata-vg`:
     ```sh
     sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
     ```
     ![Volume Group](./images/create-vg-verify.PNG)

- **Create Logical Volumes**:
   - Create `apps-lv` (14G) and `logs-lv` (remaining space) logical volumes:
     ```sh
     sudo lvcreate -n apps-lv -L 14G webdata-vg
     sudo lvcreate -n logs-lv -L 14G webdata-vg
     ```
     ![Logical Vol](./images/create-logical-volume-verfiry.PNG)

- **Format the Logical Volumes**:
   - Format with ext4 filesystem:
     ```sh
     sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
     sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
     ```
     ![Logical Fmt](./images/format-mount-logical-volume.PNG)

- **Create Directories and Mount the Volumes**:
   - Create directories:
     ```sh
     sudo mkdir -p /var/www/html
     sudo mkdir -p /home/recovery/logs
     ```
   - Mount `apps-lv` to `/var/www/html`:
     ```sh
     sudo mount /dev/webdata-vg/apps-lv /var/www/html
     ```

-  **Backup and Mount Logs**:
   - Backup log files:
     ```sh
     sudo rsync -av /var/log/. /home/recovery/logs/
     ```
   - Mount `logs-lv` to `/var/log`:
     ```sh
     sudo mount /dev/webdata-vg/logs-lv /var/log
     ```
   - Restore log files:
     ```sh
     sudo rsync -av /home/recovery/logs/log/. /var/log
     ```

- **Update fstab for Persistence**:
    - Get UUIDs: `sudo blkid`
    ![UUIDs](./images/get-uuid.PNG)

     - The highlighted code in the image above shows the `UUID` we will need to copy in a notepad. 

    - Edit `/etc/fstab`:
      ```sh
      sudo vi /etc/fstab
      ```
    - Add entries for the logical volumes `UUIDs` into the file.
        ```
        UUID=<UUID-of-apps-lv> /var/www/html ext4 defaults 0 0
        UUID=<UUID-of-logs-lv> /var/log ext4 defaults 0 0
        ```

- **Verify the Setup**:
    - Reload daemon and mount configurations:
      ```sh
      sudo mount -a
      sudo systemctl daemon-reload
      ```
      ![Reload System](./images/systemctl-reload-daemon.PNG)

    - Verify mounts: `df -h`
    ![Disk Usage](./images/check-dish-usage-verify.PNG)
