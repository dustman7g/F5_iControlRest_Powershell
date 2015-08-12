#Create a credential object 
$Username = "admin"
$SrvPassword = "Password"
$drainOption = "Drain_Option"
$node = "Node_IP" 
$pool = "PoolName"
$port = "80"
$bigIP = "bigip"
[string]$DrainID = "$($node):$($port)"


$SecPasswd = ConvertTo-SecureString "$SrvPassword" -AsPlainText -Force
$Creds = New-Object System.Management.Automation.PSCredential ("$Username", $SecPasswd)

#Disable untrusted site check
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
if($drainOption  -eq "ForceOffline"){
	$body = @{
		"state" = "user-down"
		"session" = "user-disabled"
	}
}
if($drainOption  -eq "Disable"){
	$body = @{
		"state" = "user-up"
		"session" = "user-disabled"
	}
}

if($drainOption  -eq "Enable"){
	$body = @{
		"state" = "user-up"
		"session" = "user-enabled"
	}
}

#Set the  API URI
$URI = "https://$bigIP/mgmt/tm/ltm/pool/$pool/members/~Common~$DrainID/"

Invoke-WebRequest -Method PUT -Uri "$URI"  -Credential $Creds -Body (ConvertTo-Json $body) -ContentType "application/json" -Verbose
