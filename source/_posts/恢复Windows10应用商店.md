---
title: 恢复Windows10应用商店
date: 2017-08-13T16:15:00
tags:
categories:
---

用管理员权限运行powershell,输入

```powershell
Get-AppxPackage -AllUsers| Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
```
    