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


## Description

Checks the cpu usage of syngo.plaza syngo.Common.LCMService Service
											



# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

### Body text

Lorem ipsum dolor sit amet, test link adipiscing elit. **This is strong**. Nullam dignissim convallis est. Quisque aliquam.

![Smithsonian Image](https://mmistakes.github.io/minimal-mistakes/images/3953273590_704e3899d5_m.jpg)
{: .image-right}

*This is emphasized*. Donec faucibus. Nunc iaculis suscipit dui. 53 = 125. Water is H2O. Nam sit amet sem. Aliquam libero nisi, imperdiet at, tincidunt nec, gravida vehicula, nisl. The New York Times (That’s a citation). Underline.Maecenas ornare tortor. Donec sed tellus eget sapien fringilla nonummy. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus.

HTML and CSS are our tools. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus. Praesent mattis, massa quis luctus fermentum, turpis mi volutpat justo, eu volutpat enim diam eget metus.

### Blockquotes

> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

## List Types

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### Unordered Lists

* Item one
* Item two
* Item three

## Tables

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}

## Code Snippets

{% highlight css %}
#container {
  
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
 
 
#Detect if returned object is an array
if ($usage -is [system.array]){
 
foreach ($i in $usage) {
 
$var = $usage[$count]
 
if (($var.cookedvalue/100/$CpuCores) -gt $PIDmaxUsage) {
WriteLog "ERROR: CPU TOO HIGH! Current usage "($var.cookedvalue/100/$CpuCores)"% Restarting service"
 
#restart service
try {Restart-Service $ServiceToKill}
catch {WriteLog "ERROR: Restart failed"}
}
 
else {
 
WriteLog "INFO: Process ID: $PIDname|$count is below $PIDmaxUsage. Current Usage"($var.cookedvalue/100/$CpuCores)"%"
}
$count += 1
}
}
 
else {
$var = $usage
 
if (($var.cookedvalue/100/$CpuCores) -gt $PIDmaxUsage) {
WriteLog "ERROR: CPU TOO HIGH! Current usage "($var.cookedvalue/100/$CpuCores)"% Restarting service"
 
#restart service
try {Restart-Service $ServiceToKill}
catch {WriteLog "Restart failed"}
}
 
else {
 
WriteLog "INFO: Process ID: $PIDname|$count is below $PIDmaxUsage. Current Usage"($var.cookedvalue/100/$CpuCores)"%"
}
 
}
}
{% endhighlight %}


<div markdown="0"><a href="https://github.com/kfoxirl/Moon/raw/master/assets/zips/CheckSyngoCommonLCMService.zip" class="btn btn-info">Download</a></div>


**Watch out!** You can also add notices by appending `{: .notice}` to a paragraph.
{: .notice}
