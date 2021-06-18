---
title: "Dehydration and Rehydration"
id: ""
sidebar_label: "Dehydration and Rehydration"
---
---

This process is used to rotate instances periodically or for disaster recovery when WME data(`/wm-data and /wm-runtime in directory/volumes Platform Instance and /data directory/volume in StudioWorkspace Instance / AppDeployment Instance`) is available but machines are corrupted and make sure have to use the same WME version for the recovery process.

## Data Backup

- WaveMaker Platform Stores its state into the disk. WaveMaker Platform administrators can take backups of those disk/directories and can restore them to any previous state.
- WaveMaker uses separate dedicated directory `/wm-data` in WaveMaker Platform Instance for storing data and `/data` in StudioWorkspace Instance / AppDeployment Instance.
- We move all the data to Platform Instance(/wm-data dir or volume), so that backup will be easier. No need to take backups of any (volume/dir) in any of Developer/App Deployment instances.
- Before applying the backup process do Hybernation and passivation for user and application containers using following command or do the process from launchpad.

  ```bash
  python3 /usr/local/content/wme/wme-installer/<version>/resources/python/3/passivation_deletion.py -pr <protocol> -d <domain> -u <adminUser> -p <adminPasswd>
  ```

  - **protocol** represents what web protocol is used to connect to WaveMaker application (http/https)
  - **domain** represents the domain name in which WaveMaker application is running
  - **adminUser** and **adminPasswd** refer to the admin credentials which are used to access launchpad.

  - Refer below mentioned example command for passivation

  ```bash
  python3 /usr/local/content/wme/wme-installer/10.7.1/resources/python/3/passivation_deletion.py -pr http -d wme-demo.wavemaker.com -u test@wavemaker.com -p test-password -di False
  ```

- Take a backup of `/wm-data` directory of Platform Instance by taking a snapshot of a volume.
- For the disaster and recovery process take a backup of `/data` directory of StudioWorkspace Instance / AppDeployment Instance by taking snapshots.

### AWS

- For taking a snapshot of volume in AWS cloud provider please refer [volume snapshot in aws](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html).
- Stop WME EC2 instance and detach eth1 network interface [follow the steps given here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html#detach_eni), make a note of interface ID or ENI ID
  
### Azure

- For taking a snapshot of volume in AZURE cloud provider please refer [volume snapshot in azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/snapshot-copy-managed-disk).
- Stop WME VM and detach network interface [follow the steps given here](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-vm#remove-a-network-interface-from-a-vm), make a note of interface ID.

### GCP

- For taking a snapshot of volume in GCP cloud provider please refer [volume snapshot in GCP](https://cloud.google.com/compute/docs/disks/create-snapshots).
- Stop WME VM aESXietach IP address [follow the steps given here](https://cloud.google.com/compute/docs/ip-addresses/reserve-static-internal-ip-address#deleting_a_static_internal_ip_address), make a note of interface ID.

### VMWARE ESXi and Hyper-V

- Use the below steps for taking /wm-data backup in Platform Instance, it will create data.tar in /wm-data location.
  
```bash
bash wme-installer.sh --data-archive
```

- Copy the generated data.tar to secure location on new WME Platform Instance or to any cloud storage services.

## Data restore

### Platform Instance

- Launch the instance or VM with the same IP address with the latest AMI Image. To create WME Platform Instance in the different cloud and on-premise environments please follow the below steps attach the /wm-data volume to Platform Instance and Mount the volume to the platform Instance using the following command.

#### AWS

- To launch WME Platform instance in AWS cloud environment please refer [WME Platform instance Infrastructure in AWS](/learn/on-premise/aws/wavemaker-enterprise-setup-on-aws).
  
#### AZURE

- To launch WME Platform virtual machines in AZURE cloud environmet please refer [WME Platform instance Infrastructure in AZURE](/learn/on-premise/azure/wavemaker-enterprise-setup-on-azure).
  
#### GCP

- To launch WME Platform virtual machines in GCP cloud environment please refer [WME Platform instance Infrastructure in GCP](/learn/on-premise/gcp/wavemaker-enterprise-setup-on-gcp).
  
#### VMWARE ESXI

- To create WME Platform virtual machines in VMware Esxi please refer [WME Platform instance Infrastructure in VMware Esxi](/learn/on-premise/vmware-esxi/wavemaker-enterprise-setup-on-vmware).

#### Hyper-V

- To create WME Platform virtual machines in Hyper-V please refer [WME Platform instance Infrastructure in Hyper-V](/learn/on-premise/hyper-v/wavemaker-enterprise-setup-on-hyperv).

#### Backup data mount in WME Platform Instance

```bash
mount /dev/</wm-data disk> /wm-data
example: mount /dev/xvdh /wm-data
```

- Update the fstab entry for volume on every system reboot, add an entry for the device to the /etc/fstab file.
- Take UUID of disks for identification by using the following command.

```bash
blkid
```

- To entry, the UUID of the disks in fstab, use the following format.

``` bash
UUID=</wm-data block-device-UUID>  /wm-data   ext4   defaults ,nofail  0  2
```

#### VMWARE ESXi and Hyper-V

- Copy tar which was archived in the previous steps
- Use the below steps for restore /wm-data using data.tar which was archived in previous steps in Platform Instance,

```bash
bash wme-installer.sh --data-untar
```  

[![data_untar](/learn/assets/wme-setup/upgrade-wme-setup/data-un-tar.png)](/learn/assets/wme-setup/upgrade-wme-setup/data-un-tar.png)

### StudioWorkspace Instance / AppDeployment Instance

- Launch the instance or VM with the latest AMI Image.To create WME StudioWorkspace Instance / AppDeployment Instance in the different cloud and on-premise environments please follow the below steps and attach the /data to StudioWorkspace Instance / AppDeployment Instance.

#### AWS

- To launch WME StudioWorkspace Instance / AppDeployment Instance in AWS cloud environment please refer [WME StudioWorkspace Instance / AppDeployment Instance Infrastructure in AWS](/learn/on-premise/aws/wavemaker-enterprise-setup-on-aws).
  
#### AZURE

- To launch WME Studio Workspace/AppDeploy virtual machines in AZURE cloud environmet please refer [WME StudioWorkspace Instance / AppDeployment Instance Infrastructure in AZURE](/learn/on-premise/azure/wavemaker-enterprise-setup-on-azure).
  
#### GCP

- To launch WME Studio Workspace/AppDeploy virtual machines in GCP cloud environment please refer [WME StudioWorkspace Instance / AppDeployment Instance Infrastructure in GCP](/learn/on-premise/gcp/wavemaker-enterprise-setup-on-gcp).
  
#### VMWARE ESXI

- To create WME Studio Workspace/AppDeploy virtual machines in VMware Esxi please refer [WME StudioWorkspace Instance / AppDeployment Instance Infrastructure in VMware Esxi](/learn/on-premise/vmware-esxi/wavemaker-enterprise-setup-on-vmware).

#### Hyper-V

- To create WME Studio Workspace/AppDeploy virtual machines in Hyper-V please refer [WME StudioWorkspace Instance / AppDeployment Instance Infrastructure in Hyper-V](/learn/on-premise/hyper-v/wavemaker-enterprise-setup-on-hyperv).

#### Backup data mount in StudioWorkspace Instance / AppDeployment Instance

- Mount the volume to the StudioWorkspace Instance / AppDeployment Instance using the following command.

```bash
mount /dev/</data disk> /data
example: mount /dev/xvdh /data
```

- Update the fstab entry for volume on every system reboot, add an entry for the device to the /etc/fstab file.
- Take UUID of disks for identification by using the following command.

```bash
blkid
```

- To entry the UUID of the disks in fstab, use the following format.

``` bash
UUID=</data block-device-UUID>  /data   ext4   defaults ,nofail  0  2
```

## Installing WME on New Instance

- For Download WaveMaker installation package please refer [WaveMaker package Installation](/learn/on-premise/aws/install/download-copy-installer).
- Extract Package please refer [WaveMaker package extraction](/learn/on-premise/aws/install/extract-package).
Initializing the setup please refer [WaveMaker Initialization](/learn/on-premise/aws/install/initilize-setup). Make sure to provide the same CIDR Range which is used in the previous setup.
- Setup using config wizard please refer [WaveMaker configwizard setup](/learn/on-premise/aws/install/setup-using-cw) and use same WaveMaker studio and built apps Domain names.

## Sync StudioWorkspace Instance / AppDeployment Instance

- Execute the following command in Platform Instance to sync the StudioWorkspace/AppDeploy Instances.

```bash
bash /usr/local/content/wme/wme-installer/<installler-version>/wme-installer.sh --upgrade-instances
```