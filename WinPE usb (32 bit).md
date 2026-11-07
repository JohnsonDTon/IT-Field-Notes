WinPE USB is so you can boot into a cmdline and do stuff <br>
I built this because I need to quick format hard-drives <br>
THIS ONE IS FOR OLD BIOS MACHINES. It can’t boot a 64-bit UEFI PC, so use the x64 USB for those. <br> <br>

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
copype x86 C:\WinPE_x86
mkdir C:\mount
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
robocopy C:\WinPE_x86\media P:\ /E
```


How to use USB (if cleaning a disk thats leaving a company, use clean all as it writes zeroes all over)
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

If list disk doesnt show the disk, change BIOS setting from RAID to below
```text
BIOS → SATA Operation / SATA Mode → IDE, Compatibility, or AHCI
```
