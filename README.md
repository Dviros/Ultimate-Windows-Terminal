# Ultimate-Windows-Terminal

![Terminal](https://github.com/Dviros/Ultimate-Windows-Terminal/raw/master/image.png)

## What's the Story?
I always loved clean and sleek terminals.

When Microsoft started working on Terminal last year, I got really excited that finally, Windows will have it's own unique and dynamic terminal, like Linux and macOS.

So I took Microsoft's documentations and couple of other great examples, and wanted to share my version of it.

My settings.json file contains couple of color schemes, key bindings, supports Ubuntu 20.04 and Powershell 7.


## How to?
1. Enable Windows Subsystem and restart your machine:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

2. Install Ubuntu from the Microsoft Store:

https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71

3. Install Windows Terminal from the Microsoft Store:

https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701

4. Install Cascadia Code Fonts from:

https://github.com/microsoft/cascadia-code/ -> latest version:
https://github.com/microsoft/cascadia-code/releases/download/v2005.15/CascadiaCode_2005.15.zip


5. Install Powershell 7:
```powershell
Invoke-WebRequest -URI "https://github.com/PowerShell/PowerShell/releases/download/v7.0.1/PowerShell-7.0.1-win-x64.msi" -outfile $temp\PowerShell-7.0.1-win-x64.msi
msiexec.exe /package $temp\PowerShell-7.0.1-win-x64.msi /quiet ADD_EXPLORER_CONTEXT_MENU_OPENPOWERSHELL=1 ENABLE_PSREMOTING=1 REGISTER_MANIFEST=1
```


6. Install modules and set up the profile files for Powershell 7 (execute from pwsh.exe - Powershell 7):
```powershell
C:\Program Files\PowerShell\7\pwsh.exe -Command If(-not(Get-InstalledModule posh-git -ErrorAction silentlycontinue)){;
Install-Module posh-git -Confirm:$False -Force;};

If(-not(Get-InstalledModule oh-my-posh -ErrorAction silentlycontinue)){;
    Install-Module oh-my-posh -Confirm:$False -Force;
};

If(-not(Get-InstalledModule Paradox -ErrorAction silentlycontinue)){;
    Install-Module Paradox -Confirm:$False -Force;
};
If(-not(Get-InstalledModule PSReadLine -ErrorAction silentlycontinue)){;
    Install-Module -Name PSReadLine -AllowPrerelease -Force -SkipPublisherCheck;
};

$filename = "Microsoft.PowerShell_profile.ps1";
if (!(Test-Path -Path $PROFILE)) {;
  New-Item -ItemType File -Name $filename -Path $PROFILE -Force;
  Out-file -Filepath $PROFILE\$filename "Import-Module posh-git" -Append;
  Out-file -Filepath $PROFILE\$filename "Import-Module oh-my-posh" -Append;
  Out-file -Filepath $PROFILE\$filename "Set-Theme Paradox" -Append;
};

```


7. Download the settings.json file:
```powershell
invoke-webrequest -URI "https://raw.githubusercontent.com/Dviros/Ultimate-Windows-Terminal/master/settings.json" -Out-file $env:USERPROFILE\AppData\Local\Packages\Microsoft.WindowsTerminal_*\LocalState\settings.json -Force
```
*** Manually edit (sorry) line 63: "startingDirectory": "//wsl$/Ubuntu-20.04/home/<username>" in Ubuntu and type your username.


8. Install Powerline on Ubuntu:
```bash
sudo apt install golang-go
go get -u github.com/justjanne/powerline-go
```


9. Add the following to your ~/.bashrc file. Make sure to check if GOPATH is already declared:
```bash
GOPATH=$HOME/go
function _update_ps1() {
    PS1="$($GOPATH/bin/powerline-go -error $?)"
}
if [ "$TERM" != "linux" ] && [ -f "$GOPATH/bin/powerline-go" ]; then
    PROMPT_COMMAND="_update_ps1; $PROMPT_COMMAND"
fi
```


10. Launch and enjoy:
```powershell
wt ; split-pane -p "Ubuntu-20.04 🐳" -d "//wsl$/Ubuntu-20.04/home/<username>"; split-pane -p "cmd"
```


11. Bonus Level
You can also add specific SSH commands and profiles to your settings.json file.


For example, for passwordless sign-ins, you can use Public-Key authentication with the following:
```json
	{
	"name": "SSH Connection 💻",
	"commandline": "ssh -i c:\\link\\to\\your\\key.pub username@IP",
	"icon": "C:\\cool\\icon.png",
	"acrylicOpacity" : 0.7,
        "colorScheme" : "Campbell",
	"cursorColor" : "#FFFFFD",
	"fontFace" : "Cascadia Code PL",
	"useAcrylic" : true
	}
```

And then to run:
```powershell
wt ; split-pane -p "SSH Connection"
```
Or
```powershell
wt ; new-tab -p "SSH Connection"
```

## Sources and Documentations

I highly recommend on reading Microsoft's documentations:

https://docs.microsoft.com/en-us/windows/terminal/


Sources:

https://github.com/PowerShell/PowerShell/
https://github.com/microsoft/cascadia-code/
https://github.com/justjanne/powerline-go
https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx
https://www.thomasmaurer.ch/2020/04/my-customized-windows-terminal-settings-json/
