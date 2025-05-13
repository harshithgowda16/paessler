# Lab: 03 Setting Up a PRTG Cluster

**Prerequisites:** Lab 1  
**Learning objectives:** Set up a cluster  
**Resources:** Not applicable  

## Description
In this lab, we will form a cluster using two PRTG Servers.

Detailed documentation can be found here : [**Failover Cluster step by step guide**](https://www.paessler.com/manuals/prtg/failover_cluster_step_by_step)


## Instructions

### Information to Gather
Collect the following information and store it in the table below. This prevents problems and gives you an overview of the steps you need to take prior to starting a cluster. 

| Cluster Information       | Master Node       | Failover Node     |
|---------------------------|-------------------|-------------------|
| Hostname                  |                   |                   |
| IP Address                |                   |                   |
| Windows Version           |                   |                   |
| Memory (GB)               |                   |                   |
| # CPU                     |                   |                   |
| .NET Framework Version    |                   |                   |
| PRTG Version              |                   |                   |
| License Name              |                   |                   |
| License Key               |                   |                   |

**Here is an Example Table with all the necessary information:**

| Cluster Information       | Master Node       | Failover Node     |
|---------------------------|-------------------|-------------------|
| Hostname                  | PRTGSERVER-M      | PRTGSERVER-F      |
| IP Address                | 192.168.2.12      | 192.168.2.11      |
| Windows Version           | Windows 10 Version 21H2 Build 19044.1415 | Windows 10 Version 21H2 Build 19044.1415 |
| Memory (GB)               | 6                 | 6                 |
| # CPU                     | 8                 | 8                 |
| .NET Framework Version    | 4.8.04084         | 4.8.04084         |
| PRTG Version              | 22.1.74.1712 [Canary] x64 |                   |
| License Name              | Freeware          |                   |
| License Key               | 000319-1CAKFM-8FFPVJ-GE8MD9-DT6Q3M-3KD3A8-DP2UN7-DWH0W2-1G7QJC-JACD3B* | Same as on the master node |

> ***) This is not a valid license key**

**Where to find the details:**

| Topic                  | How to find the value                                                                 |
|------------------------|---------------------------------------------------------------------------------------|
| Hostname               | Start a Command Prompt, type `HOSTNAME`                                               |
| IP Address             | Use the Command Prompt: type `IPCONFIG`, choose the IPv4 address                      |
| Windows version        | Use the Command Prompt: type `WINVER`                                                 |
| Memory (GB)            | Use the Command Prompt: type `TASKMGR` → Click the tab Performance in the TASKMGR      |
| # CPU                  | Use the Command Prompt: type `TASKMGR` → Click the tab Performance → Click CPU Graph   |
| .NET Framework Version | Command Prompt: type `REGEDIT` → Browse to: `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\NET Framework Setup\NDP\v4\Full` → Read "Version" key |
| PRTG Version           | Web Console → SETUP → System Status → Software Version and Server Information         |
| License Name           | Same as PRTG Version, but License Information instead of Software Version             |
| License Key            | Same as PRTG Version, but License Information instead of Software Version             |

## Build the Cluster

You have received three virtual machines. One runs the PRTG core server and the other runs a remote probe.

Since we don't have a spare machine, we will:
1. Move all groups back to the local probe
2. Uninstall the remote probe from the second VM and reboot
3. Clean the machine to avoid failures
4. Copy the license key and PRTG software to the second VM
5. Install PRTG on the machine that will become the failover node
6. Convert the standalone PRTG core server to the master node
7. Convert the second PRTG core server to a failover node

### Step 1: Move all groups back to the local probe

   ![](Images/LAB03/lab03-01.jpg)

1. Choose the local probe as the destination  
2. The remote probe should only have its own sensors now  

   ![](Images/LAB03/lab03-02.jpg)

### Step 2: Uninstall the remote probe from the second VM and reboot
3. On the Core: Under **Setup | System Administration | Core & Probes**, write down the **Access Key** for the remote probe  
   - Security may require adding this Access Key to the **Deny GIDs**  
4. On the VM used as Remote Probe Server:  
   - Right-click Windows **Start** menu.

   ![](Images/LAB03/lab03-03.jpg)  

   -  Click **PRTG Network Monito.r**
   -  Click **Uninstall PRTG Network Monitor.**

   ![](Images/LAB03/lab03-04.jpg)  

   - Select **Uninstall**  

   ![](Images/LAB03/lab03-05.jpg)  

   ![](Images/LAB03/lab03-06.jpg)  

   - Delete the uninstalled remote probe from the device tree  
   - Reboot the Server  

### Step 3: Clean the machine to avoid failures
5. Check:  
   - File system: Delete `C:\ProgramData\Paessler\...`  
   - Registry: Delete `HKLM\Software\WOW6432Node\Paessler\PRTG Network Monitor`  

### Step 4: Copy PRTG and the license information on the machine that will become the failover 
6. 6.	You are now still on the Server that used to be the Remote Probe: Create folder `C:\tmp`  
7. Switch to the PRTG Core Server.
8. On the first day of the training, you have downloaded PRTG from the Paessler Website. This download is in your download folder.  
   - If that download was already deleted, you can use the file that is kept in PRTG: **`C:\Program Files (x86)\PRTG Network Monitor\PRTG Installer Archive`**  
9. Copy the license key and the PRTG software to the other Server.
   - Use File Explorer: **`\\<IP address>\c$\tmp\`** \ as path to open a session on the second VM.  
   - Or use **`NET USE`** command:  

     ```cmd
     Net use S: \\<IP address>\c$\tmp\ /user:training *
     ```
   - Copy PRTG installer from download folder  

### Step 5: Install PRTG on the failover node
10. Install PRTG by double-clicking (refer to Lab INST01 if needed):  
    - Enter license information from the table (or use the copied file) you filled in 
    - Select custom installation  
    - Make sure to **Skip auto-discovery**
    - Change "**Probe Connection IP Addresses**" to "**All IP addresses available on this computer**"  
    - Choose Connection Security on the same web page. 
    - PRTG will restart because of the changed settings.
    - You do not need to log in to the web console
       - Skip the wizard to set up this PRTG core server if you do log in to the second instance.
       - You can continue with the next step, do not wait until the installation ends.

Now we have two standalone PRTG core servers using the same license, which will cause conflicts if not clustered.

## Perform these steps on the node that will be the master node
### Step 6: Convert your standalone PRTG core server to the master node of the cluster
**Saving configuration:**  

Since you want to be able to revert to a good working environment, we shall take some precautions. We will create a snapshot of your configuration. The configuration of PRTG is stored in the file PRTG Configuration.dat. This file is stored in **C:\ProgramData\Paessler\PRTG Network Monitor**. In this folder, you will also find a backup of the configuration. The extension of this file is .old. The difference between the backup and the snapshot are the changes made to the configuration between the backup and the moment you made the snapshot. (They could be the same.)

1. Under **Setup | System Administration | Administrative Tools**, click **Go!** under **Create Configuration Snapshot**.

     ![](Images/LAB03/lab03-07.jpg)  

2.	As you can read here: [Administrative tools settings](https://www.paessler.com/manuals/prtg/administrative_tools_settings)

A File is created at: **`C:\ProgramData\Paessler\PRTG Network Monitor\Configuration Auto-Backups`**  

   ![](Images/LAB03/lab03-08.jpg)  

Paessler Tech Support advises you to do this on both the PRTG core server that will become the master node as well as on every failover node. Because the configuration on the failover node will be replaced during cluster creation.

On the PRTG core server that will become the master node, we currently see that the cluster is not enabled:
  
1.	Open the PRTG web interface and log in as a PRTG administrator.
2.	Click **Setup** in the main menu bar.
3.	Under **System Administration**, check that currently no cluster exists **(Cluster (Not Enabled)).**

      ![](Images/LAB03/lab03-09.jpg)  

4. Start PRTG Administration Tool from Windows Start menu on the PRTG core server that will become the master node.   

      ![](Images/LAB03/lab03-10.jpg)  

5. Select the **Cluster** tab (1).
6.	Note that the **Cluster Mode** also indicates the status of Standalone. (2)
7.	Click the **Create a PRTG Cluster** button (3).

      ![](Images/LAB03/lab03-11.jpg)  

8.	A window will open that asks you if you want to proceed. 

      ![](Images/LAB03/lab03-12.jpg)  

9.	Click **Yes**.
10. From the **Cluster connection setup** window, write down the **Cluster Access Key**, you will need it on the failover node.
    - Leave the port for the cluster as is.

         ![](Images/LAB03/lab03-13.jpg)  

11. Click **OK** to proceed.
12. When you see a message telling you that this PRTG core server is now configured as the master node, click **OK** to restart the PRTG services.
 
   
       ![](Images/LAB03/lab03-14.jpg)  

       ![](Images/LAB03/lab03-15.jpg)  

13. After one or two minutes, open the PRTG web interface and log in as a student.
14. The first thing you should notice is that a new status icon is visible.

       ![](Images/LAB03/lab03-16.jpg)  

15. The second change you see is the new Cluster Probe in the device tree.

       ![](Images/LAB03/lab03-17.jpg)  

    - In the version used during this session the PRTG Application Server does not support a cluster yet. You might want to de-activate this
    - (Setup | Deactivate Net UI (User Interface) and New API (Alpha)

         ![](Images/LAB03/lab03-18.jpg)  

         ![](Images/LAB03/lab03-19.jpg)  

         ![](Images/LAB03/lab03-20.jpg)  

         ![](Images/LAB03/lab03-21.jpg)

         ![](Images/LAB03/lab03-22.jpg)  

    - You must delete the sensor after you can log in. The Windows Service does not exist any longer. This behavior might change as this is a Canary release and not a public release yet.

17. Go to **Setup | System Administration | Cluster.**

       ![](Images/LAB03/lab03-23.jpg)  

18. The status of the cluster is shown. Only the master node is visible, and it shows the **Active** node state.
19. Under **Setup | PRTG Status**, you will now also find **Cluster Status.**

       ![](Images/LAB03/lab03-24.jpg)  

We have successfully set up a cluster with one node. We are now ready to add a failover node. 

## Perform these steps on the node that will be the failover node
### Step 7: Convert to failover node

19. Start the **PRTG Administration Tool** from the Windows **Start** menu on the PRTG core server that will become the failover node

       ![](Images/LAB03/lab03-25.jpg)

21. Click the **Cluster** tab
22. Click "**Join a PRTG Cluster**"
23. A message pops up that asks you to confirm before proceeding. 

       ![](Images/LAB03/lab03-26.jpg)  

24. Click **Yes**
25. Fill in the details by using the table you created at the beginning of this lab and click **Join**.

       ![](Images/LAB03/lab03-27.jpg) 

       ![](Images/LAB03/lab03-28.jpg)    

26. Confirmation window appears  

       ![](Images/LAB03/lab03-29.jpg)

27. When you access the PRTG web interface, you will get the following message because the failover node has not yet been activated on the master node.

       ![](Images/LAB03/lab03-30.jpg)  

## Perform these steps on the master node
28. In the PRTG web interface, go to **Setup | System Administration | Cluster**.

       ![](Images/LAB03/lab03-31.jpg)  

29. You will see the failover node is inactive.

       ![](Images/LAB03/lab03-32.jpg)  

30. Change the failover node to **Active** and click **Save**. 
31. Both nodes should be now active. 

       ![](Images/LAB03/lab03-33.jpg)  

32. Go to **Setup | PRTG Status | Cluster Status**. (A new menu entry)  
33. Both nodes should show as active (may take time to show "**Connected**")  

      ![](Images/LAB03/lab03-34.jpg)  

**Optional:** Check cluster communication logs at:  
`C:\ProgramData\Paessler\PRTG Network Monitor\Logs\core\CoreCluster.log`

## Perform these steps on the failover node
34. PRTG takes 5-10 minutes to synchronize. Web interface shows read-only mode which you will see in the bottom-right corner.  

       ![](Images/LAB03/lab03-35.jpg)  

34. After we have set up the cluster, Switch between cluster nodes:
    
    - From master node to failover node
    - From failover node to master node
    - To do this, select **Home | Switch Cluster Node.**

         ![](Images/LAB03/lab03-36.jpg)  

- Failover node doesn't show local probe:  

     ![](Images/LAB03/lab03-37.jpg)  


## Move Devices to Cluster Probe
36. Apart from the sensors that indicate the health of the cluster probe this probe has no load, move the "**Internet Apps**" group to the Cluster Probe.
37. Repeat this for the group "**Customers**"

       ![](Images/LAB03/lab03-38.jpg)  

Before moving device/groups you need to ask the question, can the remote probes access these devices. Think about firewall rules, access restrictions on devices, VLAN helpers etc.

You have successfully created an active-active cluster with two nodes for availability purposes.

It can take a long time before the resources are visible on the Fail-over node of the cluster. This is normal behavior.

   ![](Images/LAB03/lab03-39.jpg)     

## Notes
- For XL5 licenses, add extra Failover Node columns to the information table  
- PRTG Manual: https://www.paessler.com/manuals/prtg/failover_cluster_step_by_step  
- Video: https://www.paessler.com/learn/videos/how-to-set-up-a-cluster (18 minutes)  
- If the cluster is in a failover status, you need to redirect users, maps, and API (Application Programming Interface) calls to the failover node
