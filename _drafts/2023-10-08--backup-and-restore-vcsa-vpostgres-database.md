---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: /assets/img/${filename}
typora-root-url: ../

lang: en-AU

title: KB0018
subtitle: Back up and restore vCenter Server Appliance vPostgres database
author: Stephen Schwetz

tags: 
  - KB0018
  - VCSA
  - ESX
  - vCenter
  - Knowledge Base
  - KB

updated: 
---



This article provides information for backing up and restoring the embedded vPostgres database for vCenter Server or vCenter Server Appliance. This process can be used when uninstalling the vCenter Server to prevent data loss due to the uninstalled database.

For versions before 6.x, see [Backing up and restoring the vCenter Server Appliance 5.x vPostgres database](https://kb.vmware.com/s/article/2034505).

**Note:**

* This article is only supported for backup and restore of the vPostgres database to the same vCenter Server or vCenter Server Appliance. Use of image-based backup and restore is the only solution supported for performing a full, secondary appliance restore.
* This KB article is specifically for backing up the vCenter database. Backing up and restoring your database protects the data stored in your database. The backup of the vPostgres database is not required when performing a backup using a supported method.
* vCenter Server Appliance supports a native file-based backup and restore mechanism to recover the vCenter Server Appliance after failures. For more information on this, see:
  * [File-Based Backup and Restore of vCenter Server Appliance](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.install.doc/GUID-3EAED005-B0A3-40CF-B40D-85AD247D7EA4.html) 
  * [vCenter Server Appliance File-Based Backup](https://featurewalkthrough.vmware.com/t/vsphere-6-5/vcenter-server-appliance-file-based-backup/)
  * [vCenter Server Appliance File-Based Restore](https://featurewalkthrough.vmware.com/t/vsphere-6-5/vcenter-server-appliance-file-based-restore/)
  * [Overview of Backup and Restore options in vCenter Server 6.x (2149237)](https://kb.vmware.com/s/article/2149237).

### Solution

### **Prerequisite**:

* Create a folder to store the backup and ensure read/write permissions are granted.
* Stop the vmware-vpxd and vmware-vdcs services:

**Appliance**

**6.7 and 6.5**

`service-control --stop vmware-vpxd`

`service-control --stop vmware-content-library`

 

**6.0 (Appliance)**:

`service-control --stop vmware-vpxd`
`service-control --stop vmware-vdcs`

**Windows**

Open a command prompt, navigate to C:\"Program Files"\VMware\"vCenter Server"\bin and run the following commands:

**6.7 and 6.5**:
`service-control --stop vpxd`
`service-control --stop content-library`

**6.0**:
`service-control --stop vpxd`
`service-control --stop vdcs`

### **Backing Up and Restoring the Embedded vCenter Server Appliance Database**

**Caution**: This procedure cannot be stopped. Stopping the script will cause inconsistencies in the vCenter Server appliance database and can prevent vCenter Server appliance from starting.

1. Log in to the vCenter Server Appliance Linux console as root.

2. Download the Linux backup and restore package 2091961_linux_backup_restore.zip attached to this Knowledge Base article.

3. Make a backup_lin.py executable with this command: `chmod 700 /*path_to_script*/backup_lin.py`

   For example:
   `chmod 700 /tmp/backup_lin.py`

4. Run this command: python /*path_to_script*/backup_lin.py -f /*path_to_backup*/backup_VCDB.bak

   For example:
   `python /tmp/backup_lin.py -f /tmp/backup_VCDB.bak`

When the backup completes, you see a message that the backup completed successfully.

### **Restore the vCenter Server Appliance vPostgres Database**

It may be required to move the database to a different vCenter Server Appliance VM or Windows installed vCenter Server provided it is the same vCenter Server instance as the backup source (e.g. a VM restored from backup or a clone of the existing VM). After you back up the embedded vPostgres database, you can restore it from the backup file.

**Note**: Using WinSCP on the vCenter Server Appliance may fail. For more information, see [Error when uploading files to vCenter Server Appliance using WinSCP (2107727)](https://kb.vmware.com/s/article/2107727).

**Caution**: This procedure cannot be stopped. Stopping the script will cause inconsistencies in the vCenter Server appliance database and can prevent vCenter Server appliance from starting.

1. Log in to the vCenter Server Appliance Linux console as root.

2. Download the Linux backup and restore package 2091961_linux_backup_restore.zip attached to this Knowledge Base article and extract it on the Linux machine.

3. Make restore_lin.py executable by running this commend: chmod 700 /path_to_script/restore_lin.py

   SFor example:

   `chmod 700 /tmp/restore_lin.py`

4. Stop the vmware-vpxd and vmware-vdcs services, by running these commands depending on vCenter Server version:

**6.7 and 6.5:**
service-control --stop vmware-vpxd
service-control --stop vmware-content-library

**6.0:**
service-control --stop vmware-vpxd
service-control --stop vmware-vdcs

1. Run the restore_lin.py file and provide the location for the backup file.

   For example, if the backup file is saved to /tmp/backup_VCDB.bak, run this command:

   `python /tmp/restore_lin.py -f /tmp/backup_VCDB.bak`

   When the restore completes, you see a message that the restore completed successfully.

2. Start the vmware-vpxd and vmware-vdcs services by running:

**6.7 and 6.5:**
`service-control --start vmware-vpxd`

`service-control --start vmware-content-library`

**For 6.0:**
`service-control --start vmware-vpxd`
`service-control --start vmware-vdcs`



