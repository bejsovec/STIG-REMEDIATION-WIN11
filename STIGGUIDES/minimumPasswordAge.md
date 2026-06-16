<h1>Minimum Password Age Remediation</h1>
<b>STIG ID:</b> WN11-AC-000030<br>
<b>Description:</b> The minimum password age must be configured to at least 1 day.

<h2>1. Vulnerability</h2>
<b>Issue</b><br>
The minimum password age must be configured to at least 1 day. <br><br>
<b>Risk</b><br>
Users can immediately cycle passwords to bypass history enforcement and reuse an old password.

<h2>2. Remediation</h2>

<b>Manual Remediation </b><br>
Configure the policy value for Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy.
<br> Minimum password age and set it to 1



<b>Automated Remediation (PowerShell Script)</b>
Run the following in PowerShell (or in Command Prompt) as an Administrator. 

Set minimum password age to 1 day
```
net accounts /minpwage:1
```


To check the password policy run the following in PowerShell as an Administrator. 

```
net accounts | Select-String "Minimum password age"
```
Expected Result: Minimum password age (days): 1

<img src= /IMAGES/maxsizePScheck.png>

<h2>4. Rollback Instructions (If Needed)</h2>
 Restore non-compliant setting (0 days = no minimum age) <br>

<br><b>Manual Remediation Rollback</b><br>
Configure the policy value for Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy.
<br> Minimum password age and set it to 0

<br><b>Automated Remediation (PowerShell Script)Rollback</b><br>
Run the following in PowerShell as an Administrator. This will rollback the size to the original value (20 MB).
```
net accounts /minpwage:0
```

<h2>5. Final Results</h2>

The Nessus scan shows that the vulnerability has been remediated. The user must now wait 1 day before being able to change their password.
