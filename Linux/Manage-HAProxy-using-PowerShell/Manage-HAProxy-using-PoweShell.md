
# Manage HAProxy using PowerShell

I manage a number of HAProxy CentOS VMs that are part of an Active Active pacemaker cluster. One of the routine tasks is to start/stop/drain servers from backend pools during maintenance and nightly build deployments and it is always useful to automate such tasks to save time and resources.

Below is a small PowerShell script that I use in my deployment scripts:
```
[CmdletBinding(SupportsShouldProcess=$false)]  
Param (

[Parameter(Mandatory=$false)]  
[string[]]$servers,  
[Parameter(Mandatory=$false)]  
[string]$backend,

[Parameter(Mandatory=$false)]  
[string[]]$HAProxyServers,

[Parameter(Mandatory=$false)]  
[string]$action  
)  
if ($Servers.Length -eq 1) {  
$Servers = $Servers[0].split(‘,’)  
}  
if ($HAProxyservers.Length -eq 1) {  
$Servers = $HAProxyservers[0].split(‘,’)  
}  
foreach ($HAProxyserver in $HAProxyServers)  
{

$user = “admin”  
$pass = “Pass”  
$pair = “${user}:${pass}”  
$bytes = [System.Text.Encoding]::ASCII.GetBytes($pair)  
$base64 = [System.Convert]::ToBase64String($bytes)  
$creds = “Basic $base64”  
$uri = “http://$HAProxyServer`:81”  
$headers = @{ Authorization = $creds }

foreach($server in $servers) {

$params = @{ s = $server; action = $action; b = $backend }  
Invoke-WebRequest -Uri $uri -Headers $headers -Method Post -Body $params -MaximumRedirection 0 -UseBasicParsing -ErrorAction: Stop  
}  
Write-Host $server.ToUpper() “:” $action “on” $HAProxyserver “backend:” $backend  
}
```