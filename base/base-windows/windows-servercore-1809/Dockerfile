FROM mcr.microsoft.com/windows/servercore:1809

# Install VC 10, VC 12 and VC14
RUN powershell.exe -Command \
    wget https://download.microsoft.com/download/3/2/2/3224B87F-CFA0-4E70-BDA3-3DE650EFEBA5/vcredist_x64.exe -OutFile $env:temp\\vc_redist.x64.exe; \
    Start-Process $env:temp\\vc_redist.x64.exe -ArgumentList "/quiet", "/install" -Wait; \
    Remove-Item -Force $env:temp\\vc_redist.x64.exe; \
    echo "VC10-INSTALLED"; \
    wget https://download.visualstudio.microsoft.com/download/pr/10912041/cee5d6bca2ddbcd039da727bf4acb48a/vcredist_x64.exe -OutFile $env:temp\\vc_redist.x64.exe; \
    Start-Process $env:temp\\vc_redist.x64.exe -ArgumentList "/quiet", "/install" -Wait; \
    Remove-Item -Force $env:temp\\vc_redist.x64.exe; \
    echo "VC12-INSTALLED"; \
    wget https://aka.ms/vs/16/release/VC_redist.x64.exe -OutFile:$env:temp\\VC_64.exe;\
    Start-Process $env:temp\\vc_64.exe -ArgumentList "/quiet", "/install" -Wait; \
    Remove-Item -Force $env:temp\\vc_64.exe; \
    echo "VC14-INSTALLED"
    

RUN powershell.exe -Command \
    echo "INSTALLING-CONDA"; \    
    echo "START-DOWNLOAD"; \
    wget https://repo.anaconda.com/miniconda/Miniconda3-py37_4.8.3-Windows-x86_64.exe -OutFile $env:temp\\miniconda.exe; \
    echo "DONE-DOWNLOAD"; \
    echo "START-INSTALL"; \
    Start-Process $env:temp\\miniconda.exe -ArgumentList "/S", "/InstallationType=AllUsers", "/AddToPath=1", "/D=$env:LOCALAPPDATA\\Miniconda3" -Wait; \
    echo "DONE-INSTALL"; \
    echo "REMOVE-TEMP"; \
    Remove-Item $env:temp\\miniconda.exe -Force; \
    echo "CONDA-INSTALLED";


# Install nuget.exe
RUN mkdir "C:\\nuget_tool"
RUN powershell.exe -Command \
    wget https://dist.nuget.org/win-x86-commandline/v5.7.0/nuget.exe -OutFile C:\\nuget_tool\\nuget.exe; \
    echo "nuget-INSTALLED"

# Set up the symbol link so that we can map host D drive to the d drive in the container and 
# the host F drive to F drive in the container
# create volumes
VOLUME c:/drived
VOLUME c:/mount

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices' -Name 'D:'  -Value '\??\C:\drived' -Type String;
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices' -Name 'F:'  -Value '\??\C:\mount' -Type String;

# Add DigiCertGlobalRootCA certificate
RUN powershell.exe -Command " \
    wget https://cacerts.digicert.com/DigiCertGlobalRootCA.crt -OutFile $env:temp\\DigiCertGlobalRootCA.crt; \
    Import-Certificate -FilePath $env:temp\\DigiCertGlobalRootCA.crt -CertStoreLocation Cert:\LocalMachine\AuthRoot; \
    Remove-Item $env:temp\\DigiCertGlobalRootCA.crt -Force; \
    "
# Install 7Z
RUN powershell.exe -Command \
    wget https://www.7-zip.org/a/7z2107-x64.exe -OutFile $env:temp\\7z2107-x64.exe; \
    Start-Process $env:temp\\7z2107-x64.exe -ArgumentList "/S" -Wait; \
    Remove-Item -Force $env:temp\\7z2107-x64.exe; \
    setx PATH '%PATH%;\"C:\\Program Files\\7-Zip\"'; \
    echo "7Z-INSTALLED";


# Enable the long path
RUN powershell -Command Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value "1"

CMD ["cmd"]
