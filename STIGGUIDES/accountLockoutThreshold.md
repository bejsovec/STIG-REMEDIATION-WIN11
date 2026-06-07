
<h1>Account Lockout Threshold Remediation</h1>
<b>STIG ID:</b> WN11-AC-000010<br>
<b>Description:</b> The number of allowed bad logon attempts must be configured to three or less.

<h2>1. Vulnerability</h2>
<b>Issue</b><br>
Account lockout threshold is set higher than 3 invalid logon attempts. <br><br>
<b>Risk</b><br>
Weak lockout settings could allow for a bad actor to brute force or guess a password, increasing the risk of unathorized access to a system or services. 

<h2>2. Remediation</h2>

<b>Manual Remediation </b><br>


<b>Automated Remediation (PowerShell Script)</b>
Run the following in PowerShell (or in Command Prompt) as an Administrator. This will set account lockout threshold to 3 bad logon attempts

```
net accounts /lockoutthreshold:3
```

To check the current lockout policy, run the following in PowerShell as an Administrator. 

```
net accounts | Select-String "Lockout threshold"
```
Expected Result: 

```
Lockout threshold: 3
```

Alternatively, you can check in Command Prompt as an Administrator with:

```
net accounts
```


<h2>4. Rollback Instructions (If Needed)</h2>
In order to rollback these changes, you would have first needed to check the original vaules. In Windows 11, <br>

<br><b>Manual Remediation Rollback</b><br>
Navigate back to Local Computer Policy >> Computer Configuration >> Windows Settings >> Security Settings >> Local Policies >> User Rights Assignment in local policy.<br>
Select the policy "Access this computer from the network"<br>
1. "Add User or Group" to add Administrators and Authenticated Users<br>
2. Select any other users or groups and then click remove.<br>

<br><b>Automated Remediation (PowerShell Script)Rollback</b><br>
Run the following in PowerShell as an Administrator. In Windows 22H2 and later this default value is 10 attempts and there is no lock out value in older verions. Changing to 10 will effectively change this back to the default in our Windows 11 22H2 VM.
```
Lockout threshold: 10
```

<h2>5. Final Results</h2>
<img src = "/IMAGES/WN11-AC-000010PASS.PNG">
The Nessus scan shows that the vulnerability has been remediated. An account will become locked out for 10 minutes after 3 failed attempts to login.
