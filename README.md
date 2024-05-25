# TimeMachine on DD-WRT

To set up Time Machine on a DD-WRT capable router, you'll need to configure the router to support *direct-attached storage* (DAS) and then enable *Samba*. Here's a step-by-step guide to help you through the process.

## Prerequisites

- **DD-WRT firmware installed:** Ensure your router has DD-WRT installed. You can find the firmware and installation instructions on the DD-WRT website.
- **External storage device:** You’ll need a USB drive or an external hard drive to connect to your router.
- **Basic networking knowledge:** Familiarity with DD-WRT and basic networking concepts.

## Step-by-Step Instructions

1. Partition and format an external hard drive
    - The process is out of the scope of this tutorial because there are already a lot of information available on internet.
    - In the following we are using a single EXT4 partition but it should work also with exFAT.
      
3. Connect to Your Router
    - Access your DD-WRT router’s web interface by entering the router's IP address (usually 192.168.1.1) in your web browser.
    - Log in with your admin username and password.

4. Connect and Mount the External Storage

    - Plug your USB drive or external hard drive into the router’s USB port (USB3 port is better).
    - Go to **Services > USB** in the DD-WRT web interface.
      
      **Enable** the following options:
        - *Core USB Support*
        - *USB Storage Support*
        - *Automatic Drive Mount*
        - Copy and paste UUID from Disk Info to *Mount partition to /opt*
        - Apply the settings. Your drive should be automatically mounted.
     
    ![USB](images/Services-USB.jpg)

3. Connect to Your Router via Terminal

   On macOS open a Terminal and run:
   ```
   ssh 192.168.1.1 -l root
   ```
    Enter `root` password when requested.
   
5. Intall Entware the ultimate repo for embedded devices
   
   [Entware](https://entware.net) is a software repository for embedded devices like routers or network attached storages. >1800 packages are available. It was founded as an alternative to very outdated Optware packages.
   List of Entware packages for ARMv7 is [here](http://bin.entware.net/armv7sf-k3.2/Packages.html).

   Type in the following commands:
   ```
   cd /opt
   wget http://bin.entware.net/armv7sf-k3.2/installer/generic.sh
   sh generic.sh
   ```
   When installation is complete, run an update:
   ```
   opkg update
   opkg upgrade
   ```
6. Disable the built-in Samba server
   - Go to **Services > NAS** in the DD-WRT web interface.

     **Disable** the following options:
        - *Samba Server > Samba Configuration > Enable Server*
          
        ![Samba](images/Services-NAS.jpg)
   
8. Enable Samba 


10. Add startup and shutdown script

    - Go to **Adminstration > Commands** in the DD-WRT web interface.

    Add the following to **Startup**
    ```
    echo "nogroup:x:114:nobody" >> /etc/group
    echo "nobody:*:114:114:avahi:/opt/sbin/avahi-daemon:/bin/false" >> /etc/passwd
    echo "samba:x:1000:timemachine" >> /etc/group
    echo "timemachine:*:1001:1000:TimeMachine::/bin/false" >> /etc/passwd
    /bin/sh -c 'until [ -f /opt/etc/init.d/rc.unslung ]; do sleep 1 ; done'
    /opt/etc/init.d/rc.unslung start
    for i in /dev/sd?; do hdparm -S 180 $i > /dev/null; done
    for f in /sys/class/scsi_disk/*; do echo 1 > $f/allow_restart; done
    ```

    Add  the following to **Shutdown**
    ```
    /opt/etc/init.d/rc.unslung stop
    ```
