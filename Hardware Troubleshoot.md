SSD
```powershell
Get-PhysicalDisk | Format-Table FriendlyName, MediaType, HealthStatus, OperationalStatus, Size
```
```text
1. Open Settings.
2. Go to System → Storage → Advanced storage settings → Disks & volumes.
3. Select your SSD and open Properties.
4. Look for drive health warnings, estimated remaining life, or reliability issues.
If you see a critical warning, back up your data immediately.
```
```powershell
chkdsk C: /scan
```
Storage health<br>
Look for non-zero uncorrected read and write errors, high wear, or a high temperature.
```powershell
Get-PhysicalDisk | Get-StorageReliabilityCounter | Format-Table DeviceId, Temperature, ReadErrorsUncorrected, WriteErrorsUncorrected, Wear, PowerOnHours
```


RAM
```text
Run memtest86
```
Check current speed
```powershell
Get-CimInstance Win32_PhysicalMemory | Select-Object BankLabel, Speed, ConfiguredClockSpeed
```
Check part number and see if it is compatible with said speed
```powershell
Get-CimInstance Win32_PhysicalMemory | Select-Object BankLabel, Capacity, Manufacturer, PartNumber
```

Temperature
```text
Run HWMonitor
```

Graphics Driver <br>
(Status: OK and ConfigManagerErrorCode: 0 is good)
```powershell
Get-CimInstance Win32_VideoController |
Format-List Name, DriverVersion, DriverDate, Status, ConfigManagerErrorCode
```

Check for graphics-related crash events
```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'System'
    StartTime = (Get-Date).AddDays(-7)
} -ErrorAction SilentlyContinue |
Where-Object {
    $_.ProviderName -match 'Display|nvlddmkm|amdkmdag|igfx|WHEA-Logger' -or
    $_.Message -match 'LiveKernelEvent|graphics|display driver'
} |
Select-Object TimeCreated, ProviderName, Id, LevelDisplayName, Message -First 30 |
Format-List
```

Battery
```powershell
powercfg /batteryreport
```

Check motherboard brand
```powershell
Get-CimInstance Win32_BaseBoard | Select-Object Manufacturer, Product
```
