WinPE USB is so you can boot into a cmdline and do stuff <br>
I built this because I need to format hard-drives that don't have built-in secure wipe in bios.

Download the ADK and WinPE Add-On
```text
https://learn.microsoft.com/windows-hardware/get-started/adk-install
```

Install the ADK
```text
On features, you just need Deployment Tools  ✔  (uncheck everything else)
```

Install the WinPE Add-On
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

Plug in your fresh USB. Set up the USB. Run in powershell admin
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
```
```powershell
list partition
select partition 1
format fs=fat32 quick label=WINPE
assign letter=P
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


How to use USB  (if cleaning a disk thats leaving a company, use clean all as it writes zeroes all over)
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
