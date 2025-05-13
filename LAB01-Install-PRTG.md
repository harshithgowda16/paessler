# Lab: 01  Install PRTG using the custom method 
## **Prerequisites:** None 

## Learning objectives:
- Install PRTG using the custom method
  
## Resources: not applicable  

## Description

In this lab, we will install the latest PRTG version. The PRTG installer offers two installation modes: **custom** and **express**. We want to avoid network discovery following a successful installation, so we will choose the **custom installation mode**.

**Note:** The PRTG core server might restart without prior notice once the installation is complete.  
**Note 2:** This document uses screenshots from version 96. The latest version may have different layouts.

## Instructions:

1. **Login** to the Windows machine where you plan to install the PRTG core server.

2. **Open a web browser** (Edge, Chrome, Firefox).

3. **Download PRTG** from the Paessler website:  
   [PRTG Download ᐅ Official Download - latest version](https://www.paessler.com/download/prtg-download?download=1)

4. **Run** the **PRTG_installer_with_trial_key_000023-*.exe**.  
   You can run it directly from a web browser or by navigating to **File Explorer > Downloads**.  
   As each download will generate a unique key, the name of your download is not the same.

5. **Choose English** and click **OK**. 

      ![](Images/LAB01/lab01-01.jpg)

6. Select **I accept the agreement** and then click **Next**.

      ![](Images/LAB01/lab01-02.jpg)

7. Please wait for the PRTG core server to initialize the license.

8. **Enter your email address** and click **Next**.  
   - This email address is used to send the first notification message to. When you have added other users to the product you are able to change this.  
   - If you don’t want to receive emails during the training use a non-existing email address.

      ![](Images/LAB01/lab01-03.jpg)

9. Select **Custom** as the installation mode and click **Next**. By using custom mode, the automatic discovery of network devices will be skipped.  

   **This is essential during this training.**

      ![](Images/LAB01/lab01-04.jpg)


10. Click **Next**. Your server only has one drive. In a production environment, the **Data Path** should point to a fast disc with the capacity to hold the PRTG Database.

      ![](Images/LAB01/lab01-05.jpg)

11. Select **Skip auto-discovery** and click **Next**.<br>  
    **This is essential during this training.**

      ![](Images/LAB01/lab01-06.jpg)

12. Wait until PRTG unpacks files and finishes the installation.

      ![](Images/LAB01/lab01-07.jpg)

      ![](Images/LAB01/lab01-08.jpg)

13. In the web browser that was opened by the installation, click **Log in** to access your PRTG.

      ![](Images/LAB01/lab01-09.jpg)

14. After logging in, you can operate within the PRTG user interface (UI).  
    Do **NOT** click on **OK** in the orange box with the text “Congratulations”!!!!

 - Click **Skip Introduction**.

      ![](Images/LAB01/lab01-10.jpg)

15. You have successfully installed Paessler PRTG Network Monitor.

The messages on the right side of the console will be taken care of in one of the next labs.  
Do not click them away!

### Notes:


