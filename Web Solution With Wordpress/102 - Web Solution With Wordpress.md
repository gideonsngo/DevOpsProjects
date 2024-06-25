# 102 - Web Solution With Wordpress

### Step 1: Prepare the Web Server
Launch an EC2 Instance that will serve as the "web server". Create 3 volumes in the same Availability Zone (AZ) as the web server each of 10GiB.

Attach all 3 volumes one by one to your web server EC2 instance.
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/90699fbe-bfaf-489b-bc45-66c208b44a58)

Open up the Linux Terminal to begin configuration by connecting to the instance through SSH
```
    ssh -i keypair.pem ec2-user@public-ip
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/adeca305-2c34-4102-bfee-6c1b1391fe83)

Use `lsblk` command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with `ls /dev/` and make sure you see all 3 newly created block devices there - thier names will likely be `xvdb. xvdc, xvdd`

Use `df -h` command to see all mounts and free space on your server

Use `gdisk` utility to create a single partition on each of the 3 disks:
     ```sh
     sudo gdisk /dev/xvdb
     sudo gdisk /dev/xvdc
     sudo gdisk /dev/xvdd
     ```
Use `lsblk` utility to view the newly configured partition on each of the 3 disks.
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/679f5e17-6255-4d39-937e-58672d7fb04e)

Install `lvm2` package using `sudo yum install lvm2`. Run `sudo lvmdiskscan` command to check for available partitions.

**Note:** Previously, in Ubuntu we used `apt` command to install packages, in RedHat/CentOS a different package manager is used, so we shall use `yum` command instead.

Use `pvcreate` utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM  
     ```sh
     sudo pvcreate /dev/xvdb1
     sudo pvcreate /dev/xvdc1
     sudo pvcreate /dev/xvdd1
     ```
Verify that your PV has been created successfully by running `sudo pvs`  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2f77f322-0bc5-4371-bcba-7b3adf8ced70)

Use `vgcreate` utility to add all 3 PVs to a volume group. Name the VG `webdata-vg`
     ```sh
     sudo vgcreate webdata-vg /dev/xvdb1 /dev/xvdc1 /dev/xvdd1
     ```
Verify that your VG has been created successfully by running `sudo vgs`  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ba7b32de-ede4-4622-9e8a-7a8f588e502c)

Use `lvcreate` utility to create 2 logical volumes. **apps-lv (Use half of the PV size)** and **logs-lv Use the remaining space of the PV size. NOTE:** apps-lv will be used to store data for the Website, while logs-lv will be used to store data for logs.
     ```sh
     sudo lvcreate -n apps-lv -L 14G webdata-vg
     sudo lvcreate -n logs-lv -L 14G webdata-vg
     ```
Verify that your Logical Volume has been created successfully by running `sudo lvs` 
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/bb04a154-5ed5-4c86-91ca-ffac797ac418)

Verify the entire setup
```sh
sudo cgdisplay -v #view complete setup - VG, PV and LV
sudo lsblk
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d473cc8d-e40a-4c43-b484-3814b338bb54)

Use `mkfs.ext4` to format the logical volumest with ext4 filesystem:
     ```sh
     sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
     sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
     ```
Create /var/www/html directory to store website files ```sudo mkdir -p /var/www/html```  
Create /home/recovery/logs to store backup of log data ```sudo mkdir -p /home/recovery/logs```

Mount /var/www/html on `apps-lv` logical volume 
     ```sh
     sudo mount /dev/webdata-vg/apps-lv /var/www/html
     ```
Use `rsync` utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)   
     ```sh
     sudo rsync -av /var/log/. /home/recovery/logs/
     ```
Mount /var/log on `logs-lv` logical volume.
     ```sh
     sudo mount /dev/webdata-vg/logs-lv /var/log
     ```
Restore log files back into /var/log directory  
     ```sh
     sudo rsync -av /home/recovery/logs/log/. /var/log
     ```
Update `etc/fstab` file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the `/etc/fstab` file:
 ```sudo blkid```
 ![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1c64e293-95c4-4c09-93ae-cec797a7333f)

Open `/etc/fstab` in text editor:
      ```sh
      sudo vi /etc/fstab
      ```
Update `/etc/fstab` by adding entries for the logical volumes `UUIDs` into the file.
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6f21c809-2a60-43e0-8e6c-0ec3daf50ffd)

Test the configuration and reload the daemon 

      ```sh
      sudo mount -a
      sudo systemctl daemon-reload
      ```
Verify your setup by running `df -h` output must look like this:

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8827d585-4aa4-4077-bc78-03816b7446c8)
