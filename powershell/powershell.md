\# 🟦 PowerShell Cheat Sheet



\## 1️⃣ BASICS

Get-Command

Get-Help Get-Process

Get-Help Get-Process -Examples# 🟦 PowerShell Cheat Sheet (Single Easy-Copy Block)



\## 1️⃣ BASICS

Get-Command

Get-Help Get-Process

Get-Help Get-Process -Examples

Clear-Host

Exit



Aliases (avoid in scripts):

ls   = Get-ChildItem

cd   = Set-Location

rm   = Remove-Item

cp   = Copy-Item

mv   = Move-Item



---



\## 2️⃣ NAVIGATION

Get-Location

Set-Location C:\\Temp

Get-ChildItem

Get-ChildItem -Recurse

Get-ChildItem \*.log

Get-ChildItem -File

Get-ChildItem -Directory



---



\## 3️⃣ FILES \& FOLDERS

New-Item file.txt -ItemType File

New-Item Folder1 -ItemType Directory

Remove-Item file.txt

Remove-Item Folder1 -Recurse -Force

Copy-Item a.txt b.txt

Move-Item a.txt C:\\Backup\\

Get-Content file.txt

Set-Content file.txt "Hello"

Add-Content file.txt "New line"



---



\## 4️⃣ PIPING

Get-Process | Sort-Object CPU

Get-Service | Where-Object Status -eq "Running"

Get-Process | Select-Object Name, CPU, Id



---



\## 5️⃣ FILTERING

Get-Process | Where-Object CPU -gt 100

Get-Service | Where-Object Name -like "\*sql\*"

Get-EventLog System | Where-Object EntryType -eq "Error"

Where CPU -gt 100   # short syntax



---



\## 6️⃣ SORTING \& GROUPING

Sort-Object Name

Sort-Object CPU -Descending

Get-Process | Group-Object ProcessName

Get-Process | Sort CPU -Descending | Select -First 5



---



\## 7️⃣ SYSTEM INFO

Get-Process

Get-CimInstance Win32\_Processor

Get-CimInstance Win32\_OperatingSystem

Get-PSDrive

Get-CimInstance Win32\_LogicalDisk

(Get-Date) - (Get-CimInstance Win32\_OperatingSystem).LastBootUpTime



---



\## 8️⃣ NETWORK INFO

Get-NetIPConfiguration

ipconfig

Get-NetTCPConnection

netstat -ano

Test-Connection google.com

Test-NetConnection google.com -Port 443

Resolve-DnsName google.com



---



\## 9️⃣ REAL-TIME SYSTEM ANALYSIS

Get-Process | Sort CPU -Descending | Select -First 10

while ($true) {

&nbsp; Get-Process | Sort CPU -Descending | Select -First 5

&nbsp; Start-Sleep 2

}

Get-Counter '\\Processor(\_Total)\\% Processor Time'

Get-Counter '\\Memory\\Available MBytes'



---



\## 🔟 SERVICES \& PROCESSES

Get-Service

Start-Service wuauserv

Stop-Service wuauserv

Restart-Service wuauserv

Get-Process

Stop-Process -Name notepad

Stop-Process -Id 1234



---



\## 1️⃣1️⃣ VARIABLES \& OBJECTS

$name = "Server01"

$number = 10

$p = Get-Process

$p.Name

$p.CPU



---



\## 1️⃣2️⃣ SCRIPTING

if ($CPU -gt 80) { "High CPU" } else { "Normal" }



foreach ($svc in Get-Service) {

&nbsp; $svc.Name

}



function Get-Uptime {

&nbsp; (Get-Date) - (Get-CimInstance Win32\_OperatingSystem).LastBootUpTime

}



---



\## 1️⃣3️⃣ ERROR HANDLING

Try {

&nbsp; Get-Item C:\\NoFile.txt

}

Catch {

&nbsp; "File not found"

}



---



\## 1️⃣4️⃣ EXECUTION POLICY

Get-ExecutionPolicy

Set-ExecutionPolicy RemoteSigned

.\\script.ps1



---



\## 🔥 REAL-WORLD ONE-LINERS

Get-Process | Sort CPU -Descending | Select -First 5

Get-ChildItem C:\\ -Recurse | Sort Length -Descending | Select -First 10

Get-Service | Where Status -eq "Stopped"

Get-Process | Export-Csv processes.csv -NoTypeInformation



Clear-Host

Exit



Aliases (avoid in scripts):

ls   = Get-ChildItem

cd   = Set-Location

rm   = Remove-Item

cp   = Copy-Item

mv   = Move-Item



---



\## 2️⃣ NAVIGATION

Get-Location

Set-Location C:\\Temp

Get-ChildItem

Get-ChildItem -Recurse

Get-ChildItem \*.log

Get-ChildItem -File

Get-ChildItem -Directory



---



\## 3️⃣ FILES \& FOLDERS

New-Item file.txt -ItemType File

New-Item Folder1 -ItemType Directory

Remove-Item file.txt

Remove-Item Folder1 -Recurse -Force

Copy-Item a.txt b.txt

Move-Item a.txt C:\\Backup\\

Get-Content file.txt

Set-Content file.txt "Hello"

Add-Content file.txt "New line"



---



\## 4️⃣ PIPING

Get-Process | Sort-Object CPU

Get-Service | Where-Object Status -eq "Running"

Get-Process | Select-Object Name, CPU, Id



---



\## 5️⃣ FILTERING

Get-Process | Where-Object CPU -gt 100

Get-Service | Where-Object Name -like "\*sql\*"

Get-EventLog System | Where-Object EntryType -eq "Error"

Where CPU -gt 100   # short syntax



---



\## 6️⃣ SORTING \& GROUPING

Sort-Object Name

Sort-Object CPU -Descending

Get-Process | Group-Object ProcessName

Get-Process | Sort CPU -Descending | Select -First 5



---



\## 7️⃣ SYSTEM INFO

Get-Process

Get-CimInstance Win32\_Processor

Get-CimInstance Win32\_OperatingSystem

Get-PSDrive

Get-CimInstance Win32\_LogicalDisk

(Get-Date) - (Get-CimInstance Win32\_OperatingSystem).LastBootUpTime



---



\## 8️⃣ NETWORK INFO

Get-NetIPConfiguration

ipconfig

Get-NetTCPConnection

netstat -ano

Test-Connection google.com

Test-NetConnection google.com -Port 443

Resolve-DnsName google.com



---



\## 9️⃣ REAL-TIME SYSTEM ANALYSIS

Get-Process | Sort CPU -Descending | Select -First 10

while ($true) {

&nbsp; Get-Process | Sort CPU -Descending | Select -First 5

&nbsp; Start-Sleep 2

}

Get-Counter '\\Processor(\_Total)\\% Processor Time'

Get-Counter '\\Memory\\Available MBytes'



---



\## 🔟 SERVICES \& PROCESSES

Get-Service

Start-Service wuauserv

Stop-Service wuauserv

Restart-Service wuauserv

Get-Process

Stop-Process -Name notepad

Stop-Process -Id 1234



---



\## 1️⃣1️⃣ VARIABLES \& OBJECTS

$name = "Server01"

$number = 10

$p = Get-Process

$p.Name

$p.CPU



---



\## 1️⃣2️⃣ SCRIPTING

if ($CPU -gt 80) { "High CPU" } else { "Normal" }



foreach ($svc in Get-Service) {

&nbsp; $svc.Name

}



function Get-Uptime {

&nbsp; (Get-Date) - (Get-CimInstance Win32\_OperatingSystem).LastBootUpTime

}



---



\## 1️⃣3️⃣ ERROR HANDLING

Try {

&nbsp; Get-Item C:\\NoFile.txt

}

Catch {

&nbsp; "File not found"

}



---



\## 1️⃣4️⃣ EXECUTION POLICY

Get-ExecutionPolicy

Set-ExecutionPolicy RemoteSigned

.\\script.ps1



---



\## 🔥 REAL-WORLD ONE-LINERS

Get-Process | Sort CPU -Descending | Select -First 5

Get-ChildItem C:\\ -Recurse | Sort Length -Descending | Select -First 10

Get-Service | Where Status -eq "Stopped"

Get-Process | Export-Csv processes.csv -NoTypeInformation



