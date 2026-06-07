<h1>Minimum Password Length Remediation</h1>
<b>STIG ID:</b> WN11-AC-000035<br>
<b>Description:</b> Passwords must, at a minimum, be 14 characters.

<h2>1. Vulnerability</h2>
<b>Issue</b><br>
Minimum password length is configured below 14 characters.   <br><br>
<b>Risk</b><br>
Short passwords are easier to brute-force or crack, increasing account compromise risk. 

<h2>2. Remediation</h2>

<b>Manual Remediation </b><br>
Configure the policy value for Computer Configuration >> Windows Settings >> Security Settings >> Account Policies >> Password Policy
<br>Select "Minimum password length" and change the value to 14 characters, then click okay.

<img src= /IMAGES/minpasslenAD.PNG>

<b>Automated Remediation (PowerShell Script)</b>
Set minimum password length to 14 characters
```
net accounts /minpwlen:14 
```
Verify audit policy setting with the following in PowerShell.
```
net accounts | Select-String "Minimum password length"
```
Should return: Minimum password length: 14 
<img src= /IMAGES/minpasslenADPS.PNG>

<h2>4. Rollback Instructions (If Needed)</h2>
In order to rollback these changes, you would have first needed to check the original vaules. <br>

<br><b>Manual Remediation Rollback</b><br>
Configure the policy value for Computer Configuration >> Windows Settings >> Security Settings >> Account Policies >> Password Policy
<br>Select "Minimum password length" 
1. Change to 0 characters and click Okay

<br><b>Automated Remediation (PowerShell Script)Rollback</b><br>
Restore weaker/non-compliant baseline (NOT recommended)
```
net accounts /minpwlen:0 
```
To confirm the rollback was successful:
```
net accounts | Select-String "Minimum password length"
```
<h2>5. Final Results</h2>

The Nessus scan shows that the vulnerability has been remediated. The minimum password length is now 14 characters.
<img src="/IMAGES/WN11-AC-000035PASS.PNG">
