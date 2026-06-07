
<h1>Secure Remote Access Remediation</h1>
<b>STIG ID:</b> WN11-UR-000010<br>
<b>Description:</b> The 'Access this computer from the network' user right must only be assigned to the Administrators and Remote Desktop Users groups.

<h2>1. Vulnerability</h2>
<b>Issue</b><br>
The <b>"Access this computer from the network"</b> right is assigned to unauthorized users or groups. This allows more accounts than necessary to access the system over the network. 
This is not considered a finding if a management tool requires a domain service account to remotely manage systems.<br><br>
<b>Risk</b><br>
Unauthorized network access increases the risk of lateral movement and data exposure. A compromised account could be used to gain access to system resources. 

<h2>2. Remediation</h2>

<b>Manual Remediation </b><br>
Manual remediation to be completed by editing local policy. In an enterprise environment the same thing would best be accomplished using group policy.<br><br>
Navigate to Local Computer Policy >> Computer Configuration >> Windows Settings >> Security Settings >> Local Policies >> User Rights Assignment.<br>
Select the policy "Access this computer from the network"
1. "Add User or Group" to add Administrators and Remote Desktop Users
2. Select any unathorized users or groups and then click remove.

<img src = "https://github.com/bejsovec/STIG-REMEDIATION-WIN11/blob/main/IMAGES/secureRemoteGPO.PNG">

<b>Automated Remediation (PowerShell Script)</b>
Run the following in PowerShell as an Administrator. This will create a configuration file that allows only Administrators and Authenticated users to access this computer remotely.
```
$policyPath = "$env:TEMP\policy.inf"
@"
[Version]
signature="`$CHICAGO`$"

[Privilege Rights]
SeNetworkLogonRight = *S-1-5-32-544,*S-1-5-32-555
"@ | Out-File -FilePath $policyPath -Encoding ASCII
```
After creating the configuration file, it must be applied.
```
secedit /configure /db "$env:TEMP\secpol.sdb" /cfg $policyPath /areas USER_RIGHTS
```
<img src ="https://github.com/bejsovec/STIG-REMEDIATION-WIN11/blob/main/IMAGES/secureRemoteAccessPSscript.PNG">
<h2>3. Testing / Verification</h2> 
Run the following in PowerShell as an Administrator:

```
(secedit /export /areas USER_RIGHTS /cfg "$env:TEMP\secpol.cfg" | Out-Null); Get-Content "$env:TEMP\secpol.cfg" | Select-String "SeNetworkLogonRight"; Remove-Item "$env:TEMP\secpol.cfg"
```
Expected Result:
```
SeNetworkLogonRight = *S-1-5-32-544,*S-1-5-32-555
```
The results for this display the SID, the ones displayed above are mapped to the following groups:
S-1-5-32-544 = Administrators
S-1-5-32-555 = Remote Desktop Users


<h2>4. Rollback Instructions (If Needed)</h2>
In order to rollback these changes, you would have first needed to check the original vaules. In Windows 11, the deafult values for the policy allow Administrators and Authenticated Users access.<br>
<br><b>Manual Remediation Rollback</b><br>
Navigate back to Local Computer Policy >> Computer Configuration >> Windows Settings >> Security Settings >> Local Policies >> User Rights Assignment in local policy.<br>
Select the policy "Access this computer from the network"<br>
1. "Add User or Group" to add Administrators and Authenticated Users<br>
2. Select any other users or groups and then click remove.<br>

<br><b>Automated Remediation (PowerShell Script)Rollback</b><br>
Run the following in PowerShell as an Administrator:
```
$cfg = @"
[Unicode]
Unicode=yes
[Privilege Rights]
SeNetworkLogonRight = *S-1-5-32-544,*S-1-5-11
[Version]
signature="`$CHICAGO`$"
Revision=1
"@
$cfg | Out-File -FilePath C:\rollback.cfg -Encoding Unicode
```
S-1-5-32-544 and S-1-5-11 are the SIDs for Administrators and Authenticated Users.<br>

Expected Result:
```
secedit /configure /db C:\Windows\security\local.sdb /cfg C:\rollback.cfg /areas USER_RIGHTS /quiet
```

<h2>5. Final Results</h2>
<img src = "/IMAGES/WN11-UR-000010PASS.PNG">
The Nessus scan shows that the vulnerability has been remediated. Now only user accounts that are in the Administrators or Remote Desktop Users will be able to remotely login to the system.
