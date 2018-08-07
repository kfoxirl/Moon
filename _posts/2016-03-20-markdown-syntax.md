---
layout: post
title:  "ChecksyngoCommonLCMService.ps1"
date:   2017-03-16
project: true
excerpt: "Checks the cpu usage of syngo.plaza syngo.Common.LCMService Service."
tag:
- markdown 
- syntax
- sample
- test
- jekyll
comments: true
---

<figure>
	<img src="https://github.com/kfoxirl/Moon/blob/master/assets/img/powershellicon.png?raw=true">
	
</figure>


## Script Description

Checks the cpu usage of syngo.plaza syngo.Common.LCMService Service and restarts as needed.

## History

|Version | Date       | Author    | Comments                                                                                      |
|:-------|:----------:|----------:|----------------------------------------------------------------------------------------------:|
| 1.00   | 2017-03-16 | Karl Fox  | Implemented to address issue at Crystal Run with Max CPU usage.                               |
| 1.01   | 2017-03-17 | Karl Fox  | Changed $PIDName value to WmiPrvSE based on clarification from SE who handled initial call.   |
| 1.02   | 2017-03-17 | Karl Fox  | Changed to WMI Measurement of CPU Usage instead of CPU time from Get-Process.                 |
|==================================================================================================================================
{: rules="groups"}

## Code Snippet

{% highlight css %}
  
Param() 

Set-StrictMode -Version "Latest"

# Include Section
function IncludeScript($FileName){ 
	$Invocation = (Get-Variable MyInvocation -Scope 1).Value 
	return  Join-Path (Split-Path $Invocation.MyCommand.Path) $FileName -resolve
} 
.(IncludeScript "..\Include\WatchdogCommonLibrary.ps1")


$erroractionpreference = "SilentlyContinue"


### GLOBAL VARIABLES ###
$myVersion = "1.02"
$myEvntLogScriptName = "Watchdog plug-in [ChecksyngoCommonLCMService v$myVersion]"
$myScriptPath = $MyInvocation.MyCommand.path
$myConfigFile = $myScriptPath + ".config.xml"
$myCommandLine = [System.Environment]::CommandLine
$myPid = $PID
$myLogFile = $myScriptPath + ".log"
$myProtocol = $null
$myPort = $null
$mySqlServerAddress = $null
$myDbInstance = $null
$myLogInfo = ""
$aString = ""



$PIDname = "WmiPrvSE"
$ServiceToKill = "syngo.Common.LCMService"
$count = 0
$PIDmaxUsage = 30
$CpuCores = (Get-WMIObject Win32_ComputerSystem).NumberOfLogicalProcessors 
$usage = Get-Counter -ComputerName localhost '\Process(wmiprvse)\% Processor Time' `
    | Select-Object -ExpandProperty countersamples `
    | Select-Object -Property instancename, cookedvalue `
    | Sort-Object -Property cookedvalue -Descending | Select-Object -First 20 `
 
 

{% endhighlight %}


<div markdown="0"><a href="https://github.com/kfoxirl/Moon/raw/master/assets/zips/CheckSyngoCommonLCMService.zip" class="btn btn-info">Download</a></div>


**Watch out!** There are two versions of the Watchdog Plugin for Standard Central Server and a Clustered Central Server.
{: .notice}
