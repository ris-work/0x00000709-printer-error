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

## Nuclear Option
