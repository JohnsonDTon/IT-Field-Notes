WinPE USB is so you can boot into a cmdline and do stuff <br>
I built this because I need to quick format hard-drives

Download the 2004 ADK and its WinPE add-on<br>
Both are listed under “Other ADK downloads” on Microsoft’s ADK page.
```text
https://learn.microsoft.com/windows-hardware/get-started/adk-install
→ Other ADK downloads → Windows 10, version 2004)
```

Install the ADK (note, if you already installed the 64 bit, you gotta uninstalled that via Installed Apps screen)
```text
On features, you just need Deployment Tools  ✔  (uncheck everything else)
```

Install the WinPE Add-On (note, if you already installed the 64 bit, you gotta uninstalled that via Installed Apps screen)
```text
Accept Defaults
```

Create the WinPE working files. Run in admin 'Deployment and Imaging Tools Environment'
```powershell
copype amd64 C:\WinPE_amd64
mkdir C:\mount
```

Download HP Client Windows PE 10 x64 Driver Pack
```text
https://ftp.hp.com/pub/caps-softpaq/cmit/softpaq/WinPE10.html
```

Extract the drivers
```text
It will ask location. Choose this location
C:\Drivers\HP-WinPE
```

Mount WinPE and add the HP Drivers. Run in admin 'Deployment and Imaging Tools Environment'
```powershell
Dism /Mount-Image /ImageFile:C:\WinPE_amd64\media\sources\boot.wim /Index:1 /MountDir:C:\mount
Dism /Add-Driver /Image:C:\mount /Driver:C:\Drivers\HP-WinPE /Recurse
Dism /Unmount-Image /MountDir:C:\mount /Commit
```

Plug in your fresh USB. Set up the USB. Run in cmdline
```powershell
diskpart
list disk
```

Select disk (may not be 2, so check)
```powershell
select disk 2
detail disk
```

Create partition for WinPE
```powershell
clean
create partition primary size=2048
assign letter=P
format fs=fat32 quick label=WINPE
```

Create storage partition for remaining space
```powershell
create partition primary
assign letter=S
format fs=ntfs quick label=STORAGE
exit
```

Copy files into USB
```powershell
bootsect /nt60 P: /force /mbr
robocopy C:\WinPE_amd64\media P:\ /E
```


How to use USB
```powershell
diskpart
list disk
select disk 0
detail disk
clean
exit
wpeutil shutdown
```

Short version is
```powershell
diskpart
list disk
select disk 0
clean
```
