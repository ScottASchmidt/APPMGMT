# APPMGMT
Unzip the file to a local directory on the target machine (For MECM integration please perform this task on the MECM server or a workstation where the cmdlets are enabled / installed
Production script is AppImport_1_2.ps1

Pre-Req:
1. Create a content directory for source and target apps (EG: c:\AppSource
2. Create a sub directory for MSI apps under source (EG: c:\Appsource\MSI
3. Create a Target directory for content to be created (EG: c:\Applications)
4. Create a sub directory for each integration type (eg: C:\Applications\MEM  or c:\applications\SCCM)

Source Content setup:
Copy MSI files over to the MSI Source directory
- Each MSI package and it's associated content should reside in this directory (c:\appsource\MSI\Chrome)
- BUG ALERT *** - If there is more than one MST or MSP file in the root directory, please remove them (this is a bug that I will fix eventually). 
                  Tool will automatically append in switch params for MST and MSP - but will break if there is more than 1 of each
Notes:
- When usinc MECM integration specify target path as UNC
- When using MDT or MEM integration specify source and target path as local path
- For SCCM - Change lines 169 and 170 to reflect your env. (Will add this as a param eventually) 
Parameters:
MDTDeploymentPath - Path to deployment share
SourcePath - Path to source files (reference pre-req 1)
TargetPath - Path to create target application files (Reference pre-req 3)
IntuneUserName - must be in UPN format
TenantName - tenantname.onmicrosoft.com
IntergrationBackend - Pre-populated with "SCCM", "MDT", "Intune"

Script uses pre-created batch templates to aid with logging the install / uninstall procedures
- Install_template.cmd
- Uninstall_template.cmd

EX for Intune - AppImport_1.2.ps1 -SourcePath C:\IntuneApps\Source -TargetPath C:\IntuneApps\Applications -TenantName pm.onmicrosoft.com -IntuneUserName ITA@pm.onmicrosoft.com -IntergrationBackend Intune

EX for SCCM - AppImport_1.2.ps1 -SourcePath C:\IntuneApps\Source -TargetPath \\\Server1\c$\Applications\SCCM -IntergrationBackend SCCM

Intune Integration Notes:
- Script will pause after creating the install / uninstall wrappers to allow for editing prior to Win32 app packaging
- Script will prompt for Intune authentication if a valid token does not exist
