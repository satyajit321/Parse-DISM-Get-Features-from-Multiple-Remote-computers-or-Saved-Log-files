<p><strong>.Synopsis</strong><br /> &nbsp;&nbsp; Collect Windows Features Info using DISM and Parse the string data into filterable PS Object. Across multiple computers.<br /> &nbsp;&nbsp; Or Use existing DISM output to parse the data without collecting<br /> <strong>.DESCRIPTION</strong><br /> &nbsp;&nbsp; This is useful when we don't have Get-WindowsFeature available on older Windows but have PowerShell.<br /> &nbsp;&nbsp; The output is in PSObject format which can be formatted easily.<br /> &nbsp;&nbsp; It can be used to fetch local or remote computer data.<br /> &nbsp;&nbsp; It automatically copies a current DISM raw data in a logfile.<br /> &nbsp;&nbsp;&nbsp;&nbsp; Can be used to re-filter the data directly, without having to fetch the data again and again.</p>
<p>I have used the below script as a reference on the logic.<br /> #Windows - Ensure Features Installed<br /> #https://library.octopusdeploy.com/#!/step-template/actiontemplate-windows-ensure-features-installed<br /> &nbsp;&nbsp; <br /></p>
<p>The scripts&nbsp;uses 'Dism /online /Get-Features'&nbsp; command to get the data using Invoke-Command. Each computer raw data is also stored in a individual overwritable dism_&lt;computername&gt;.log file for future reference.</p>
<p><a id="143267" href="/scriptcenter/site/view/file/143267/1/Dism_Server1.log">Dism_Server1.log</a></p>
<p><a id="143268" href="/scriptcenter/site/view/file/143268/1/Dism_Server2.log">Dism_Server2.log</a></p>
<p>Most importantly the remote computers names are included in the output, just as a normal cmdlet PSremoting shows it.</p>
<p>&nbsp;</p>
<p>Refer to the help and other options usage on the details below.</p>
<p>help .\Get-DISMRemoteFeatures.ps1</p>
<p>&nbsp;</p>
<p>Use this to auto direct to the online link for this script<br /> help .\Get-DISMRemoteFeatures.ps1 -Online</p>
<div class="endscriptcode">&nbsp;</div>
<div class="endscriptcode">
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>PowerShell</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">powershell</span>
<pre class="hidden">    -------------------------- EXAMPLE 1 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1
    
             
    
    
    
    -------------------------- EXAMPLE 2 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 | ft -AutoSize
    
    
           
    
    
    
    -------------------------- EXAMPLE 3 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 | ?{$_.State -eq 'Enabled'} | ft -AutoSize
    
    
    Feature                                     State  
    -------                                     -----  
    WindowsRemoteManagement                     Enabled
    TelnetClient                                Enabled
    WindowsGadgetPlatform                       Enabled
    MediaPlayback                               Enabled
    
    
    
    
    
    -------------------------- EXAMPLE 4 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 -UseLog | ?{$_.State -eq 'Disabled'} | ft -AutoSize
    
    
This can be used as a second time re-run, which will result in faster execution as it will be refering to dism.log file.
    Make sure script has run atleast once without this(-UseLog) switch.
    
    
    
    
    
    -------------------------- EXAMPLE 5 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 | ?{$_.Feature -like "Tel*"}
    
    
    Feature                                                                State                                                                
    -------                                                                -----                                                                
    TelnetClient                                                           Enabled                                                              
    TelnetServer                                                           Disabled
    
    Filtering the output using wildcards (*)
    
    
    
    
    
    -------------------------- EXAMPLE 6 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 -Computers Server1,Server2 -UseLog | ft -AutoSize
    
    
    Calling multiple computers at a time.
    
    
    
    
    
    -------------------------- EXAMPLE 7 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 -UseLog -Computers Server1,Server2 | ?{$_.Feature -like "Tel*"}
    
    
    State                                   ComputerName                            Feature
    -----                                   ------------                            -------
    Enabled                                 Server1                              TelnetClient
    Disabled                                Server1                              TelnetServer
    Enabled                                 Server2                              TelnetClient
    Disabled                                Server2                              TelnetServer
    
    In this example, we are reading the logs Dism_Server1.log,Dism_Server2.log which was generated automatically.
    
    
    
    
    
    -------------------------- EXAMPLE 8 --------------------------
    
    C:\PS&gt;Parse existing logs\ DISM output it needs to be starting with 'Dism_'. Like 'Dism_Filename1.log','Dism_Filename2.log' can be parsed as below.
    
    
    .\Get-DISMRemoteFeatures.ps1 -UseLog -Computers FileName1,FileName2 | ?{$_.Feature -like "Tel*"}
    
State                                   ComputerName                            Feature
-----                                   ------------                            -------
Enabled                                 FileName1                               TelnetClient
Disabled                                FileName1                               TelnetServer
Enabled                                 FileName2                               TelnetClient
Disabled                                FileName2                               TelnetServer
    
    
    
    
    
    -------------------------- EXAMPLE 9 --------------------------
    
    C:\PS&gt;.\Get-DISMRemoteFeatures.ps1 -Computers (Get-Content ComputerList.txt)
    
    
    Use this to pass a list of Computers using a txt file.
    
    ComputerList.txt
    Server1
    Server2</pre>
<div class="preview">
<pre class="powershell">&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;1&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;2&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">|</span>&nbsp;<span class="powerShell__alias">ft</span>&nbsp;<span class="powerShell__operator">-</span>AutoSize&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;3&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">|</span>&nbsp;?{<span class="powerShell__variable">$_</span>.State&nbsp;<span class="powerShell__operator">-</span>eq&nbsp;<span class="powerShell__string">'Enabled'</span>}&nbsp;<span class="powerShell__operator">|</span>&nbsp;<span class="powerShell__alias">ft</span>&nbsp;<span class="powerShell__operator">-</span>AutoSize&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Feature&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;State&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;WindowsRemoteManagement&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;TelnetClient&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;WindowsGadgetPlatform&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;MediaPlayback&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;4&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">-</span>UseLog&nbsp;<span class="powerShell__operator">|</span>&nbsp;?{<span class="powerShell__variable">$_</span>.State&nbsp;<span class="powerShell__operator">-</span>eq&nbsp;<span class="powerShell__string">'Disabled'</span>}&nbsp;<span class="powerShell__operator">|</span>&nbsp;<span class="powerShell__alias">ft</span>&nbsp;<span class="powerShell__operator">-</span>AutoSize&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
This&nbsp;can&nbsp;be&nbsp;used&nbsp;as&nbsp;a&nbsp;second&nbsp;time&nbsp;re<span class="powerShell__operator">-</span>run,&nbsp;which&nbsp;will&nbsp;result&nbsp;<span class="powerShell__keyword">in</span>&nbsp;faster&nbsp;execution&nbsp;as&nbsp;it&nbsp;will&nbsp;be&nbsp;refering&nbsp;to&nbsp;dism.log&nbsp;file.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Make&nbsp;sure&nbsp;script&nbsp;has&nbsp;run&nbsp;atleast&nbsp;once&nbsp;without&nbsp;this(<span class="powerShell__operator">-</span>UseLog)&nbsp;<span class="powerShell__keyword">switch</span>.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;5&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">|</span>&nbsp;?{<span class="powerShell__variable">$_</span>.Feature&nbsp;<span class="powerShell__operator">-</span>like&nbsp;<span class="powerShell__string">"Tel*"</span>}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Feature&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;State&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;TelnetClient&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;TelnetServer&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Disabled&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Filtering&nbsp;the&nbsp;output&nbsp;using&nbsp;wildcards&nbsp;(<span class="powerShell__operator">*</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;6&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">-</span>Computers&nbsp;Server1,Server2&nbsp;<span class="powerShell__operator">-</span>UseLog&nbsp;<span class="powerShell__operator">|</span>&nbsp;<span class="powerShell__alias">ft</span>&nbsp;<span class="powerShell__operator">-</span>AutoSize&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Calling&nbsp;multiple&nbsp;computers&nbsp;at&nbsp;a&nbsp;time.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;7&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">-</span>UseLog&nbsp;<span class="powerShell__operator">-</span>Computers&nbsp;Server1,Server2&nbsp;<span class="powerShell__operator">|</span>&nbsp;?{<span class="powerShell__variable">$_</span>.Feature&nbsp;<span class="powerShell__operator">-</span>like&nbsp;<span class="powerShell__string">"Tel*"</span>}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;State&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ComputerName&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Feature&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetClient&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Disabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetServer&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Enabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetClient&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Disabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetServer&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__keyword">In</span>&nbsp;this&nbsp;example,&nbsp;we&nbsp;are&nbsp;reading&nbsp;the&nbsp;logs&nbsp;Dism_Server1.log,Dism_Server2.log&nbsp;which&nbsp;was&nbsp;generated&nbsp;automatically.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;8&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;Parse&nbsp;existing&nbsp;logs\&nbsp;DISM&nbsp;output&nbsp;it&nbsp;needs&nbsp;to&nbsp;be&nbsp;starting&nbsp;with&nbsp;<span class="powerShell__string">'Dism_'</span>.&nbsp;Like&nbsp;<span class="powerShell__string">'Dism_Filename1.log'</span>,<span class="powerShell__string">'Dism_Filename2.log'</span>&nbsp;can&nbsp;be&nbsp;parsed&nbsp;as&nbsp;below.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">-</span>UseLog&nbsp;<span class="powerShell__operator">-</span>Computers&nbsp;FileName1,FileName2&nbsp;<span class="powerShell__operator">|</span>&nbsp;?{<span class="powerShell__variable">$_</span>.Feature&nbsp;<span class="powerShell__operator">-</span>like&nbsp;<span class="powerShell__string">"Tel*"</span>}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
State&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ComputerName&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Feature&nbsp;
<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
Enabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileName1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetClient&nbsp;
Disabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileName1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetServer&nbsp;
Enabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileName2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetClient&nbsp;
Disabled&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileName2&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TelnetServer&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;EXAMPLE&nbsp;9&nbsp;<span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span><span class="powerShell__operator">-</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;C:\<span class="powerShell__alias">PS</span>&gt;.\Get<span class="powerShell__operator">-</span>DISMRemoteFeatures.ps1&nbsp;<span class="powerShell__operator">-</span>Computers&nbsp;(<span class="powerShell__cmdlets">Get-Content</span>&nbsp;ComputerList.txt)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Use&nbsp;this&nbsp;to&nbsp;pass&nbsp;a&nbsp;list&nbsp;of&nbsp;Computers&nbsp;using&nbsp;a&nbsp;txt&nbsp;file.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;ComputerList.txt&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Server1&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;Server2</pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
&nbsp;</div>
