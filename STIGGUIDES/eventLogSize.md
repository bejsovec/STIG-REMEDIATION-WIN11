<h1>Event Log Size Remediation</h1>
<b>STIG ID:</b> WN11-AU-000500<br>
<b>Description:</b> The Application event log size must be configured to 32768 KB or greater.

<h2>1. Vulnerability</h2>
<b>Issue</b><br>
The Application event log maximum size is configured below 32768 KB. <br><br>
<b>Risk</b><br>
Insufficient log size can cause event loss and reduce forensic visibility during incidents. 

<h2>2. Remediation</h2>

<b>Manual Remediation </b><br>
Configure the policy value for Computer Configuration > Administrative Templates > Windows Components > Event Log Service > Application 
<br>Select the policy 'Specify the maximum log file size (KB)' to 'Enabled' with a 'Maximum Log Size (KB)' of '32768' or greater. 

<img src= /IMAGES/maxSizeGPO.png>

<b>Automated Remediation (PowerShell Script)</b>
Run the following in PowerShell (or in Command Prompt) as an Administrator. 

This will set Application log max size to 32MB (32768 KB)
```
wevtutil sl Application /ms:1052672
```
<img src= /IMAGES/maxsizePS.png>

To check the current Application log max size, run the following in PowerShell as an Administrator. 

```
wevtutil gl Application | Select-String "maxSize"
```
Expected Result: 1052672 (or the size that was chosen)

<img src= /IMAGES/maxsizePScheck.png>

<h2>4. Rollback Instructions (If Needed)</h2>
In order to rollback these changes, you would have first needed to check the original vaules. <br>

<br><b>Manual Remediation Rollback</b><br>
Configure the policy value for Computer Configuration > Administrative Templates > Windows Components > Event Log Service > Application 
<br>Select the policy 'Specify the maximum log file size (KB)' to 'Enabled' with a 'Maximum Log Size (KB)' of '32768' or greater. <br>
1. Select Not Configured and click Okay

<br><b>Automated Remediation (PowerShell Script)Rollback</b><br>
Run the following in PowerShell as an Administrator. This will rollback the size to the original value (20 MB).
```
wevtutil sl Application /ms:20971520   # 20 MB in bytes
```

<h2>5. Final Results</h2>

The Nessus scan shows that the vulnerability has been remediated. The application event log size has been increased.
