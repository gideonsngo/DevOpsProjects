# 102 - DevOps Tooling Website Solution
## Step 1 - Prepare NFS Server
- **Launch an EC2 Instance**:
   - Create an EC2 instance with RHEL 8 Operating System that will serve as the `NFS server`. (Note: Only works with minimum m3.medium instance type)  
     <img width="669" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/636dade4-a0f2-4afc-bfc8-741e756878df">  
   - Create three 10GiB volumes in the same Availability Zone (AZ) as the `NFS server`.
     <img width="911" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d686789a-54d6-4ca0-b4fa-d14ec40190ba">  
   - Attach the volumes to the `NFS server`.

- **Open the Linux Terminal**:
   - **SSH into the instance**
  
    ```
    ssh -i keypair.pem ec2-user@public-ip
    ```
    <img width="432" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/628a581f-1b2e-425e-a71d-a75b43ccd501">

   - Inspect attached block devices: `lsblk`  
     <img width="260" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/31c12c89-f8fa-4fd4-997b-d89f00bc58ea">

   - Check all mounts and free space: `df -h`  
     <img width="306" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c2fe75a7-9a7f-4bdd-a781-c0333804cea8">

- **Create Partitions on Each Disk**:
   - Use `gdisk` to create a single partition on each of the three disks:
     ```sh
     sudo gdisk /dev/xvdb
     sudo gdisk /dev/xvdc
     sudo gdisk /dev/xvdd
     ```
     <img width="299" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ac4232a8-25ee-4dbd-be74-a124e2b9cbbd">

      1. Type `n` to create new partition.
      2. Type `1` to select numbers of new partition.
      3. Click on `Enter` to accept default settings till it prompts for a command again.
      4. Type `w` to write.
      5. Then `y` to overwrite the partition.
      6. Repeat same process for other disks

    - Inspect attached block devices: `lsblk`  
        <img width="253" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4adb2d40-e1ee-4452-a1ab-2e210859aeb7">

- **Configure LVM on the Server**:
   - First we install the `lvm2` service.
     ```sh
     sudo yum install lvm2 -y
     sudo lvmdiskscan
     ```
     <img width="442" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7adae6f0-cf12-4361-b835-1a2679a9f24b"> <img width="282" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c6c9f172-6525-4349-8840-d1db2f508dc1">
  
   - Create physical volumes, a volume group, and logical volumes:
   
     ```sh
     sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
     sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
     sudo lvcreate -n opt-lv -L 10G webdata-vg
     sudo lvcreate -n apps-lv -L 10G webdata-vg
     sudo lvcreate -n logs-lv -L 10G webdata-vg
     ```
     <img width="445" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/fc73935e-aa78-4bb8-a6cd-4aebba5a93ef">

   - Format the logical volumes with `xfs`:
     ```sh
     sudo mkfs.xfs /dev/webdata-vg/opt-lv
     sudo mkfs.xfs /dev/webdata-vg/apps-lv
     sudo mkfs.xfs /dev/webdata-vg/logs-lv
     ```
     <img width="436" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f2b07761-537f-404f-9552-42af9c7b7034">

   - Create mount points and mount the logical volumes:
     ```sh
     sudo mkdir -p /mnt/apps /mnt/logs /mnt/opt
     sudo mount /dev/webdata-vg/apps-lv /mnt/apps
     sudo mount /dev/webdata-vg/logs-lv /mnt/logs
     sudo mount /dev/webdata-vg/opt-lv /mnt/opt
     ```
     <img width="419" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ce046536-a306-458e-b1ee-12f43fbdccf1">

- **Install and Configure NFS Server**:
   - Install NFS server packages and start the service:
     ```sh
     sudo yum update -y
     sudo yum install nfs-utils -y
     sudo systemctl start nfs-server.service
     sudo systemctl enable nfs-server.service
     sudo systemctl status nfs-server.service
     ```
     <img width="441" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/eba8e12d-6a2a-496c-97d0-87f41345e150">

   -  Export the mounts for webservers' `subnet cidr` to connect as clients. Check your `subnet cidr` by opening your EC2 details in AWS web console and locate 'Networking' tab and open a Subnet link:
     <img width="619" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c43aec27-725e-4453-9cdd-df404e3bb134">
     
   -  Configure NFS exports:
     ```sh
     sudo vi /etc/exports
     ```
     Add the following lines:
     ```sh
     /mnt/apps 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
     /mnt/logs 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
     /mnt/opt 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
     ```
     <img width="392" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ccc3eabb-d443-4618-9c4e-6fdc5d7d4cea">

     Save and apply the changes:
     ```sh
     sudo exportfs -arv
     ```
     <img width="271" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ba9c5eeb-45da-40ef-b8c9-04121f492d67">

        - Set permissions for the mount points:
     ```sh
     sudo chown -R nobody: /mnt/apps /mnt/logs /mnt/opt
     sudo chmod -R 777 /mnt/apps /mnt/logs /mnt/opt
     sudo systemctl restart nfs-server.service
     ```
     <img width="437" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c0aa592d-c518-40aa-8ede-0bbcf8de5777">

- **Open NFS Ports**:
   - Check NFS ports and open them in the security group:
     ```sh
     rpcinfo -p | grep nfs
     ```
     <img width="308" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1f87a3f3-1016-4e2c-ba01-d1db79f5ff74">

   - Open ports TCP 111, UDP 111, and UDP 2049 in the security group.
    <img width="681" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1dcedec8-8f7a-4532-9d9f-8d0cd68ae056">
