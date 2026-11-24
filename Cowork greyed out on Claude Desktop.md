The error is "Cowork requires Claude Desktop be installed with our modern installer" and Claude still stays in machine even if you uninstall it under "Installed Apps" settings <br>
Run these commands when the user is logged in! <br>
This documentation is still in progress! <br>

Stop all process of Claude
```powershell
Get-Process *claude* | Stop-Process -Force
```

Remove the MSIX package for a clean install
```powershell
Get-AppxPackage -AllUsers *Claude* | Remove-AppxPackage -AllUsers
```

Uninstall Claude from the Settings menu
```text
Settings > Apps > Installed apps > Claude > Uninstall
```

Run this command to uninstall Claude 
```powershell
& "$env:LOCALAPPDATA\AnthropicClaude\Update.exe" --uninstall
```

Check for leftovers
```powershell
Test-Path "$env:LOCALAPPDATA\AnthropicClaude"
Test-Path "$env:APPDATA\Claude"
```

Remove any that returns true
```powershell
Remove-Item "$env:LOCALAPPDATA\AnthropicClaude" -Recurse -Force
Remove-Item "$env:APPDATA\Claude" -Recurse -Force
```

Confirm its all gone
```powershell
Get-AppxPackage -AllUsers *Claude*
Test-Path "$env:LOCALAPPDATA\AnthropicClaude"
```

Check if Virtual Machine Platform Windows Features is enabled
```powershell
Get-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
```

Enable it if its disabled
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -All -NoRestart
Restart-Computer
```


Download and install Claude
```text
https://claude.com/download -> Save to C:\Temp (make it if folder doesnt exist) -> Run in admin
```

Check what Claude you have
```powershell
Get-StartApps *Claude*
```

Launch the MSIX version directly (Change this to the ID of your Claude!)
```powershell
Start-Process "shell:AppsFolder\Claude_pzs8sxrjxfjjc!Claude"
```
(GUI ALTERNATIVE)
```text
WIN+R -> shell:AppsFolder -> doubleclick Claude
```


---
Test his one <br>

Uninstall Claude from the Settings menu
```text
Settings > Apps > Installed apps > Claude > Uninstall
```

Fallback command to uninstall Claude. I run both just in case, Claude is persistent.
```powershell
& "$env:LOCALAPPDATA\AnthropicClaude\Update.exe" --uninstall
```

Check if Virtual Machine Platform Windows Features is enabled
```powershell
Get-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
```

Enable it if its disabled
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform -All -NoRestart
Restart-Computer
```
Download and install Claude
```text
https://claude.com/download -> Save to C:\Temp (make it if folder doesnt exist) -> Run in admin, Input admin credentials when UAC is prompted.
```

Check what Claude you have
```powershell
Get-StartApps *Claude*
```

Launch the MSIX version directly (Change this to the ID of your Claude!)
```powershell
Start-Process "shell:AppsFolder\Claude_pzs8sxrjxfjjc!Claude"
```
(GUI ALTERNATIVE)
```text
WIN+R -> shell:AppsFolder -> doubleclick Claude
```
