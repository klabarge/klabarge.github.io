---
layout: post
title:  "March and April 2019 - Microsoft Breaks Custom URI Schemes"
date:   2019-04-15 17:20:21 -0400
categories: jekyll update
---

### Background
I perform Windows Updates for my company. We use a 3rd part tool, Ivanti, to manage and deploy patches. Our update schedule starts the day after patch Tuesday (due to our timezone, patches are typically released late Tuesdsay).

As you can imagine, this doesn't leave us much time to test the Windows updates before deploying. I find myself (more and more) having to rollback an update to the machines.

### The Problem...
This March, we found that the monthly rollup broke a URI handler on Windows 10 for one of our in-house apps. This feature was used often enough where I needed to remove the problematic patches.

**Side note:** If you don't know what a URI handler is, the quickest way to explain is by example. Type `mailto:email@test.com` in your browser. This should open up your default mail app, addressed to email@test.com.

In April, the patch Microsoft released to address this problem did not fix the issue, and broke this on Windows 7 computers for us :disappointed:

### Remediation

I couldn't find a comprehensive list anywhere on the internet, so I've compiled a list of patches (below) that we had to uninstall. These KB #'s are for both Windows 7 and Windows 10.

I'm a big fan of PowerShell, so I'm always finding excuses to learn more `cmdlets`. This little situation allowed me to get acquainted with `Get-HotFix`.

I didn't need to get elaborate with this (this time), as most of the removal was handled through Ivanti, however when we missed a computer, this was handy for our helpdesk to not have to guess which patches were installed.

```powershell
[array]$updates = "KB4490481",
"KB4482887",
"KB4489889",
"KB4489873",
"KB4489892",
"KB4490481",
"KB4493435",
"KB4493472",
"KB4493474",
"KB4493509"

[array]$computers = "ComputerName1",
"ComputerName2",
"ComputerName3",

Get-HotFix -id $updates -computername $computers
```

#### Sample Output

```
Source        Description      HotFixID      InstalledBy          InstalledOn              
------        -----------      --------      -----------          -----------              
ComputerName1 Update           KB4482887     NT AUTHORITY\SYSTEM  4/12/2019 12:00:00 AM    
ComputerName2 Update           KB4490481     NT AUTHORITY\SYSTEM  4/12/2019 12:00:00 AM    
ComputerName3 Update           KB4490481     NT AUTHORITY\SYSTEM  4/12/2019 12:00:00 AM    
```