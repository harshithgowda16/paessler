# Lab: 02  Install Classic Remote Probe
### **Prerequisites:** Lab 1  

## Learning objectives:
- Prepare the PRTG core server  
- Install a classic remote probe  
- Offload the remote probe  

**Resources:** not applicable  

## Description:
In this lab you will install a classic remote probe on a separate machine and connect it with your PRTG Core server.  

The core server is configured to work with only a local probe. Therefore, we must change settings and restart PRTG.  

**Note:** You cannot install any additional probes on the PRTG core server!  

## Instructions:

### Prepare the PRTG core server
After installation of PRTG, the local probe runs on the core. PRTG is configured to only listen on IP address 127.0.0.1. As the remote probe does not work with that address, we must reconfigure PRTG to make is possible to work with classic remote probes and multi-platform probes.

1. Connect to the PRTG core server.  
2. Log in using an administrator account (Student or prtgadmin).  
3. Select **Setup | System Administration | Core & Probes.**

     ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-1.jpg)
 
4. Under Probe Connection IPs:  
   - Select **Specify IP addresses** 
   - Check both addresses 
   - Under Allow IPs, enter **any**.
**(More secure is to add the actual IP address of the Remote Probe server)**  
   - Set Connection Security level to **High security (TLS 1.3, TLS 1.2)**.  

       ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-2.jpg)

5. Click **Save**.  
- Click **OK** to restart the PRTG core server.  

     ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-3.jpg)

6. Make a note the IP address of the core to use on the second Virtual Machine.  
   - In your production environment it is preferred to use the DNS name.
  
7. Once the PRTG web interface is available, switch to your second VM (Virtual Machine).

## Install a classic remote probe:

We have the option to download the probe software and then copy it to the location where we want it.  

The easier way is to log on to the device that will be a remote probe for PRTG and start a web browser with which we connect to the core. This we will do in the next section of this lab.  

**In the next step, you must connect to the second virtual machine!!!**  

8. Connect to your second virtual machine.  
9. Log in to your second VM.  
- Username: `training`
- Password: `prtg4training`
10. Open your web browser (Google Chrome recommended) and connect to your PRTG core server by entering the IP address (see step 6 of this lab).  
    - Our ReadyTech environment does not offer a DNS solution, that is why you must use the IP address of your PRTG Core Server.  
    - Use **HTTPS** (in Lab3 you enabled SSL), not http.

> **Note:** Use your VM's IP address, not the one shown in this example screenshot.

   ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-4.jpg)

11. Log into PRTG using the Student or PRTGadmin user account.  
12. Select **Setup | Optional Downloads | Classic Remote Probe Installer**.  

       ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-5.jpg)

- Else from the Setup page.  

     ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-6.jpg)

     ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-7.jpg)

13. We have two options to install the Classic Remote Probe:  
    - Use a wizard; a UI guides you to setup the Classic Remote Probe. This is the easiest method.  
    - Download the software and manually take the required steps. This method is often used when going to the physical location (i.e. Customers of your company) you then can take the software with you on a transportable medium.  
    Click **Add Classic Remote Probe**.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-8.jpg)

14. By clicking **Prepare and Download** you confirm you are on the right device. The remote probe installer is downloaded.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-9.jpg)

**Please keep in mind that you cannot install a remote probe on the core!!!** 

**The above screenshot says at least Windows 7; This will be correct in one of the next releases as Windows 10 is the minimum supported version.**  

15. Depending on the web browser you see the probe is being downloaded.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-10.jpg)

16. Use the File Explorer to get to the folder **Downloads**.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-11.jpg)

17. The file `PRTG_Remote_Probe_Installer_for_<IP-address>_with_key_{<key>}.exe` must be executed to install the software for the Remote Probe.  
    - The IP address can also be a DNS (Domain Name System) name; it depends on how you access the browser. **(In the training environment, we rely on the IP address.)**
    - The file can be used within your organization for all probes, more secure is to generate such file for each Remote Probe.  
    - If you install remote Probes in customer site, you **should** use a fresh generated installer per customer.  
18. Run the remote probe installer (during this training your user is member of the administrators' group) and then click **Install** and wait until the installation is done.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-12.jpg)

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-13.jpg)

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-14.jpg)

-  NPCAP is part of the installation, used for auto-discoveries in the networks of the probe.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-15.jpg)

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-16.jpg)

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-17.jpg)

19. When you see the above screen: click **Continue**.  

> **Note:**  Sometimes this step shows a failure. Often traffic is blocked => modify firewalls
> 
> Or
> 
> The DNS name cannot be resolved, solve the DNS issue or use the IP-address. 

20. Click **Finish**.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-18.jpg)

**If you have problems, use the back button to retry, after you solve the cause of the problem. If needed ask the instructor.**

21. Please return to your web browser where you have started the probe installation.  

    Once the remote probe has been installed, click **Approve**  
    → This is essential for this training  

    (only approve the new remote probe) or **Approve and auto-discover** (approve the remote probe and perform the auto-discovery).

       ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-19.jpg)

- **Approve** adds the probe without a network discovery.
- **Approve and auto-discover** adds the probe then starts a network discovery. This causes a lot of traffic in the training environment:

   **DO NOT USE THIS OPTION IN THIS TRAINING**

- Until the button is clicked, this message will be show on all PRTG consoles where the user has write-access to the root object.  

     ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-20.jpg)

- The device tree also offers buttons to approve or deny:

     ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-21.jpg)

22. You might prefer to indicate that the device is a remote probe, use the rename function to achieve this.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-22.jpg)

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-23.jpg)

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-24.jpg)

23. Now that we have more than one probe, you can see that the objects have been grouped under the probe.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-25.jpg)

24. Under **SETUP | PRTG Status | Probes** you now see the 2 probes connected to your PRTG Core.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-26.jpg)


## Offload a Probe
One of the reasons to start a remote probe is to offload another probe. Paessler recommends to not use the local probe for monitoring in a configuration with 5000 sensor and more.  
This training environment does not hold so many sensors. The next steps, however, give you the procedure to move sensors from one probe to another.  

25. It is not relevant whether you execute these steps in the console on your VM with the PRTG Core or the VM with the Classic Remote Probe!  

    Open the **Devices** menu in your console; if the Local Probe is collapsed, expand it. You should be able to see the Internet Apps group as well as the LAB17Device.  

-   Do for both objects: right-click and choose **Move | To Other Group**.

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-27.jpg)

26. Choose your remote probe to move the selected object to.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-28.jpg)

Depending on the objects beneath the selected object you see an overview of the dependent objects. To be sure you should review this section, so you eventually can move specific objects first if needed.  

27. Your remote probe should now look like this when you collapse the objects.  

      ![INSTALL CLASSIC REMOTE PROBE](Images/LAB02/lab01-29.jpg)


## Keep in mind
- The LAB17Device was configured with IP address 127.0.0.1  
  - This address is also accessible by the remote probe; the sensors keep working.  
- The Internet Apps group hold devices on the Internet. In a production environment that probably is not the case. It might happen that access lists on switches or VLAN's do not allow traffic between the probe server and the moved devices. You had better check that before moving the objects, but you could of course always move the objects back in case you forgot, and errors start to pop up.  
- If you use sensors that depend on scripts, oidlibs (SNMP Library file for PRTG) etcetera, you need to manually copy them first to the destination probe.  

## Notes:
To uninstall a remote probe, see the PRTG Manual: [https://www.paessler.com/manuals/prtg/uninstall](https://www.paessler.com/manuals/prtg/uninstall)  

**Reasons why the remote probe is not installed correctly:**  
- Network connectivity.  
- Firewall blocks the PRTG Probe port (23560).  
- DNS resolution is not working correctly.  
