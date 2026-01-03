# 0x00000709-printer-error
Solving the 0x00000709 printer error, especially with Microsoft Windows (R) 11, accessing it remotely through SMB (Windows File and Printer Sharing). Options to fix it, all in one place - what I've collected, so that you don't have to do it again. Please read the Nuclear Option too, in case nothing else works; it is easier than I thought and what actually worked for me.

## (1) Allow Insecure Guest Logins
This setting can restrict it from BOTH SIDES, so apply it to both.
[AllowInsecureGuestAuth](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/microsoft-windows-workstationservice-allowinsecureguestauth?source=recommendations)  
[How to enable insecure guest logons in SMB2 and SMB3](https://learn.microsoft.com/en-us/windows-server/storage/file-server/enable-insecure-guest-logons-smb2-and-smb3?tabs=group-policy)  

## (2) Enable Printer Browsing
[Printer-related settings](https://learn.microsoft.com/en-us/troubleshoot/windows-server/printing/use-group-policy-to-control-ad-printer)
There, it describes ways to enable Printer Browsing.

## (3) RPC failures
This is there is gpedit.msc but if you are in Windows 10/11 Home Edition, there won't be a Group Policy Editor for you, so you have to edit the relevant registry settings.
[RPC connection updates for print in Windows 11](https://learn.microsoft.com/en-us/troubleshoot/windows-client/printing/windows-11-rpc-connection-updates-for-print)  
Try setting it on BOTH SIDES to Named Pipes only, and if it doesn't work, try TCP in that case ALSO DEFINE AN RPC PORT AND EXEPMT IT FROM WINDOWS FIREWALL (`wf.msc`).

## Nuclear Option (will always work)
You need to dedicate about 512MiB RAM for this. Install VirtualBox. Then install minimal Debian in a VM (not Desktop). Should need less than 10GiB space. Should be fast enough. Once installation is done, shut down, replace the network adapter with a bridged one with the current connection. Inside the VM, edit the network configuration (Usually `/etc/network/interfaces`), set things appropriately and allocate a static local IP for easy success. Install CUPS. Now, in the VM settings, create a USB filter (USB section) and add the printer there. Now when the VM boots again (not reboot, just boot after shutdown), you should see the USB attached to the VM instead. Install the appropriate drivers. Now add the printer in CUPS. Enable printer sharing in CUPS (set Allow From All in the configuration). Once this is done, the printer should be auto-discovered by Microsoft Windows (R) 11 when you click "Add Printer".

As a bonus, this should also be auto-discovered with iPhones as well. On Android(TM) phones, you can add the URL manually like ipp://[ip]:631/printers/PRINTER_NAME. Now your printer JUST WORKS, everywhere.

### Note on paper sizes
On Microsoft Windows, there are some limitations on paper sizes, so not all papers are recognized. A4, etc. should work with the standard "IPP Class Driver", but if it's a receipt printer, it worked for me with 29.70x7.0cm. It doesn't take widths more than 7cm for similar heights. If the prints are not what you expected, try setting minimal right/left margins (0.1cm, etc.).

### Lessons learnt
#### IPP is superior because
 - It just works, with Microsoft Windows (R), Android (R), Apple (R) iOS
 - Auto-discovery
 - Non-proprietary
 - Should just work with FreeBSD and GNU/Linux as well, not tested yet

