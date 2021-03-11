---
layout: post
title: "PowerShell - WARNING: Unable to find module repositories."
categories: design
author:
- Daniel
published: true
---

If you find yourself working in a company where a proxy is setup to control traffic, you might see the above error for a various of reasons.

I was trying to install an external module Posh-Git when I got an error about PowerShellGet couldn't find any repositories or something along those lines.

```powershell
PowerShellGet\Install-Module posh-git -Scope CurrentUser
```

Then I started to fuck around with these commands, and gosh I just find PowerShell verbose and straight-up a pain to write.

```powershell
Register-PSRepository -Default

Get-PSRepository
```

Still no luck. Same warning and no repositories.

I am of course, these days, behind a big fat proxy where I cannot even download the most simple tools for making my day more smooth. 

So I thought that it must be the proxy that caused the fuckery.

Let's see what is going on.

```cmd
netsh winhttp show proxy
```

Bingo! It outputted a proxy server "frost.fyi.prx:80"

Then I simply added the proxy to this to my profile script: "C:\Users\YOUR-NAME\Documents\WindowsPowerShell\profile.ps1", like so

```powershell
[system.net.webrequest]::defaultwebproxy = new-object system.net.webproxy('http://frost.fyi.prx:80')
[system.net.webrequest]::defaultwebproxy.credentials = [System.Net.CredentialCache]::DefaultNetworkCredentials
[system.net.webrequest]::defaultwebproxy.BypassProxyOnLocal = $true
```

Then restart PowerShell and run

```powershell
Register-PSRepository -Default

Get-PSRepository
```

And you should be home free