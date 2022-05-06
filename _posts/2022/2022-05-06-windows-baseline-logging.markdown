---
layout: "post"
title: "Windows Baseline Logging"
date: "2022-05-06 23:29"
---
Pretty much a reduced version/summary of a real nice writeup by [nullsec](https://nullsec.us/windows-baseline-logging/)

# Sysmon
- [Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Swift on Security configuration](https://github.com/SwiftOnSecurity/sysmon-config)
- Place under C:\Windos\Sysmon
- Start service
  - `C:\Windos\Sysmon\Sysmon.exe -accepteula -i c:\Windows\Sysmon\sysmonconfig-export.xml`
- Update configuration of it has been changed
  - `C:\Windows\Sysmon\Sysmon.exe -c c:\Windows\Sysmon\sysmonconfig-export.xml`

# Local Policy Logging Settings
Hit `Start` and search for `MMC` ([Microsoft Management Console](https://docs.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/what-is-microsoft-management-console))

Find your way to and configure as suggested:
```
Computer Configuration\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Detailed Tracking

    Audit PNP Activity
      Success
    Audit Process Creation
      Success and Failure

Computer Configuration\Administrative Templates\System\Audit Process Creation
    Include command line process creation events
      Enabled

Computer Configuration\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Account Management
    Audit Other Account Management Events
      Success and Failure
    Audit Security Group Management
      Success and Failure
    Audit User Account Management
      Success and Failure

Computer Configuration\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Logon/Logoff
    Audit Account Lockout
        Success and Failure
    Audit Logoff
        Success
    Audit Logon
        Success and Failure
    Audit Other Logon/Logoff Events
        Success and Failure
    Audit Special Logon
        Success and Failure

Computer Configuration\Windows Settings\Security Settings\Advanced Audit Policy Configuration\Object Access
    Audit Other Object Access Events
        Success and Failure
```
# PowerShell Logging
Only works with PowerShell 5 or higher

PSTranscription logs will not purge automatically based on any volume or time and may fill up the drive. Suggested configuration also place them in a writeable location. Changing this to a write only file share, or rather picking them up via Splunk and make a scheduled task to periodically purge the directory
```
Get-ChildItem C:\PSTranscription -Recurse | Where-Object { $_.LastWriteTime -lt (Get-Date).AddHours(-1) } | Remove-Item –Recurse
```
Suggestion:
```
Computer Configuration\Administrative Templates\Windows Components\Windows PowerShell
    Turn on Module Logging
        Enabled
        Click "Show" next to Module Names and enter * in the value field to record all modules
    Turn on PowerShell Script Block Logging
        Enabled
    Turn on PowerShell Transcription
        Enabled
        Enter "C:\PSTranscription" for the output directory
        Check Include Invocation Headers
```
Add "`profile.ps1`" file containing the following lines to `C:\Windows\System32\WindowsPowerShell\v1.0`
```
$LogCommandHealthEvent = $true
$LogCommandLifecycleEvent = $true
```
# Firewall Logging
Each profile gets its own log (`domain_pfirewall.log`, `private_pfirewall.log`, `public_pfirewall.log`) both dropped and successful connections. CHange names for each profile.

If this is configured via Group Policy, it should be configured in "`Computer Configuration\Policies\Windows Security\Security Settings\Windows Firewall with Advanced Security settings`" NOT the pre-Win 2003 location of "`Network Templates\Network Connections`".

# Links
- [Logging Cheat Sheets](https://www.malwarearchaeology.com/cheat-sheets)
- [Windows Security Monitoring](https://docs.google.com/spreadsheets/d/1BhR3cymZ53ZJfJdKAGKszuB-jgsr8GBJBOCJl50WGKE/edit#gid=1983305745)