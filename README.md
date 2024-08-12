#Performing Local System Administration with PowerShell

## Exercise 1: Creating and Managing Active Directory Objects

### Task 1: Create a New Organizational Unit (OU) for a Branch Office

New-ADOrganizationalUnit -Name London

### Task 2: Create a Group for Branch Office Administrators
powershell
New-ADGroup "London Admins" -GroupScope Global

### Task 3: Create a User and Computer Account for the Branch Office
powershell
New-ADUser -Name Ty -DisplayName "Ty Carlson"
Add-ADGroupMember "London Admins" -Members Ty
New-ADComputer LON-CL2

## Task 4: Move the Group, User, and Computer Accounts to the Branch Office OU
powershell
Move-ADObject -Identity "CN=London Admins,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
Move-ADObject -Identity "CN=Ty,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
powershell
Move-ADObject -Identity "CN=LON-CL2,CN=Computers,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"

## Exercise 2: Configuring Network Settings on Windows Server

### Task 1: Test the Network Connection and Review the Configuration
powershell
Test-Connection LON-DC1
Get-NetIPConfiguration

### Task 2: Change the Server IP Address
powershell
New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.15 -PrefixLength 16
Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11

### Task 3: Change the DNS Settings and Default Gateway for the Server
Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 172.16.0.12
Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false
New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2

### Task 4: Verify and Test the Changes
powershell
Get-NetIPConfiguration
Test-Connection LON-DC1

## Exercise 3: Creating a Website

### Task 1: Install the Web Server (IIS) Role on the Server
powershell
Install-WindowsFeature Web-Server

### Task 2: Create a Folder on the Server for the Website Files
New-Item C:\inetpub\wwwroot\London -Type directory

### Task 3: Create the IIS Website
powershell
Copy code
New-IISSite London -PhysicalPath C:\inetpub\wwwroot\London -BindingInformation "172.16.0.15:8080:"
