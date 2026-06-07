<h1>Logon/Logoff Audit Remediation</h1>
<b>STIG ID:</b> WN11-AU-000054<br>
<b>Description:</b> The system must be configured to audit Logon/Logoff - Account Lockout failures.

<h2>1. Vulnerability</h2>
<b>Issue</b><br>
Audit policy for Account Lockout failures under Logon/Logoff is not enabled.  <br><br>
<b>Risk</b><br>
Failed logon tracking gaps reduce ability to detect brute force and password attack activity. 

<h2>2. Remediation</h2>

<b>Manual Remediation </b><br>
Configure the policy value for Computer Configuration >> Windows Settings >> Security Settings >> Advanced Audit Policy Configuration >> System Audit Policies >> Logon/Logoff  
<br>Select the policy 'Audit Account Lockout' select 'Failure' and click Okay.

<img src= /IMAGES/loginfailureaudit.PNG>

<b>Automated Remediation (PowerShell Script)</b>
Enable auditing for Account Lockout failures.
```
auditpol /set /subcategory:"Account Lockout" /failure:enable
```
Verify audit policy setting with the following in PowerShell.
```
auditpol /get /subcategory:"Account Lockout"
```
<img src= /IMAGES/loginfailureauditPS.PNG>

<h2>4. Rollback Instructions (If Needed)</h2>
In order to rollback these changes, you would have first needed to check the original vaules. <br>

<br><b>Manual Remediation Rollback</b><br>
Configure the policy value for Computer Configuration >> Windows Settings >> Security Settings >> Advanced Audit Policy Configuration >> System Audit Policies >> Logon/Logoff  
<br>Select the policy 'Audit Account Lockout'
1. Select Not Configured and click Okay

<br><b>Automated Remediation (PowerShell Script)Rollback</b><br>
Run the following in PowerShell as an Administrator. This will disable the auditing.
```
auditpol /set /subcategory:"Account Lockout" /failure:disable
```

<h2>5. Final Results</h2>

The Nessus scan shows that the vulnerability has been remediated. The device now keeps an aduit log of all Account Lockouts.
<img src="/IMAGES/loginfailureauditTENABLE.PNG">
