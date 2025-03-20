# SentinelX

This is SentinelX a penetration testing attack vm, specifically designed for Windows Server 2016 and above.

to deploy:

```powershell
# Ensure PowerShell Execution Policy allows running scripts
Set-ExecutionPolicy Bypass -Scope Process -Force

# Check for Administrator Privileges
if (-Not ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Host "Please run this script as Administrator!" -ForegroundColor Red
    Exit
}

# Prompt for Credentials
$Cred = Get-Credential -Message "Enter administrator credentials for SentinelX setup"

# Install Boxstarter
Write-Host "Downloading and Installing Boxstarter..." -ForegroundColor Cyan
. { Invoke-WebRequest -useb https://boxstarter.org/bootstrapper.ps1 } | iex; Get-Boxstarter -Force

# Validate URL before proceeding
$BoxstarterURL = "https://raw.githubusercontent.com/adcouch/SentinelX/main/windply/sentinelx_deploy.choco"
try {
    Invoke-WebRequest -Uri $BoxstarterURL -UseBasicParsing | Out-Null
} catch {
    Write-Host "Error: Unable to reach deployment script at $BoxstarterURL" -ForegroundColor Red
    Exit
}

# Install SentinelX Package
Write-Host "Installing SentinelX via Boxstarter..." -ForegroundColor Green
Install-BoxstarterPackage -PackageName $BoxstarterURL -Credential $Cred

```
