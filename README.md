# DD-WRT-TimeMachine

To set up Time Machine on a DD-WRT capable router, you'll need to configure the router to support network-attached storage (NAS) and then enable Samba. Here's a step-by-step guide to help you through the process.

## Prerequisites

- DD-WRT firmware installed: Ensure your router has DD-WRT installed. You can find the firmware and installation instructions on the DD-WRT website.
- External storage device: You’ll need a USB drive or an external hard drive to connect to your router.
- Basic networking knowledge: Familiarity with DD-WRT and basic networking concepts.

## Step-by-Step Instructions

1. Connect to Your Router
    - Access your DD-WRT router’s web interface by entering the router's IP address (usually 192.168.1.1) in your web browser.
    - Log in with your admin username and password.

2. Connect and Mount the External Storage

    - Plug your USB drive or external hard drive into the router’s USB port.
    - Go to Services > USB in the DD-WRT web interface.
      Enable the following options:
        - Core USB Support
        - USB Storage Support
        - Automatic Drive Mount
        - Apply the settings. Your drive should be automatically mounted. Note the mount point (usually /mnt).

3. Connect to Your Router via Terminal

    >ssh 192.168.1.1 -l root

4. Intall EntWare

   - 
6. Enable Samba 

If you want to use SMB/CIFS alongside AFP, you can enable Samba for Windows file sharing.

Go to Services > NAS.
Under Samba, enable the service and configure the shared path (e.g., /mnt).
Set a shared name and configure user access.
