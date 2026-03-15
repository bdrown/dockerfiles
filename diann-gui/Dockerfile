FROM mcr.microsoft.com/windows/nanoserver:ltsc2025

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

#Copy installers
RUN (New-Object System.Net.WebClient).DownloadFile('https://github.com/vdemichev/DiaNN/releases/download/2.0/DIA-NN-2.1.0-Academia.msi', 'c:\tools\DIA-NN-2.1.0-Academia.msi') ;\
    Start-Process 'msiexec' -ArgumentList '/i c:\tools\DIA-NN-2.1.0-Academia.msi /quiet /qn /norestart /log c:\tools\installLib.log';\
    Start-Sleep -s 30 ;\
    Remove-Item c:\tools\*.msi -force