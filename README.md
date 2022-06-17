# MS-MSDT_Office_RCE_Follina

<p> Hello Infosec/IT Admin reader, by the time I'm writing this you've already heard of this new 0-day vulnerability in Microsoft Office (CVE-2022-30190).
  Basically a remote code execution exists when MSDT - Microsoft Support Diagnostic Tool is called using the URL from application Word.</p>
  
 # Exploit DIY
  <ol>
    <li> Create word document .docx and import (OLE) object as Bitmap Image and save it</li>
    <li> Open .docx file as archive and copy over files <strong>\word\document.xml</strong> and <strong>\word\_rels\document.xml.rels</strong></li>
    <li> Now we need to edit file <strong>"document.xml.rels"</strong> and look for <strong>Target="embeddings/oleObject1.bin"</strong> , this needs to be changed to your remote location of xxxx.html file and add new parameter <strong>TargetMode="External"</strong></li>
    </br>
   
![image](https://user-images.githubusercontent.com/85706972/171281149-4c0cdb77-dad8-448d-88bd-296722999a7b.png)

    
<li>Now we need to edit second file <strong>"document.xml"</strong> and look for following paramter <strong>"OLEObject"</strong> there we need to change <strong>Type="Embed"</strong> to <strong>Type="Link"</strong> and add new parameter <strong>"UpdateMode="Oncall"</strong> and <strong>ProgID="htmlfile"</strong></li>
  </br>
   
![image](https://user-images.githubusercontent.com/85706972/171281018-a4d69fac-fab3-4323-865c-bf6981fedefc.png)

<li>Download <strong>index.html</strong> payload and I used python to create http server, from which payload will be downloaded and executed.</li>

<li>Before lunching don't forget to put back both files that we extracted and edited back to .docx with 7-zip, so exploit could work </li>
</br>

![image](https://user-images.githubusercontent.com/85706972/171277755-a05c0901-37ab-4d49-9b2f-45869ec9bd64.png)

<li> Lunch the .docx file </li>
  
 </ol>
 
 ![image](https://user-images.githubusercontent.com/85706972/171279127-decbbeff-a8d0-4d0f-af5e-c15b657fd05c.png)

<p><li> As soon as we execute we can see that file is downloaded from our local hosted server and it opens msdt.exe which then calls our calc.exe. This worked for me on Office ProPlus LTSC 2021 version, but I've seen posts about working exploit on Office365
  
# Hunting 
 
  <p> <li>To start hunt the possible exploit in environment we can use sigma rule - https://gist.github.com/matthewB-huntress/14ab9d309f25a05fc9305a8e7f351089 . For example if you can't search using ready sigma rule you can utilize sigmac that will create search query for your SIEM solution.
    Also we can use Process Explorer to see what is process is started from WINWORD and how calc.exe was opened
    
![image](https://user-images.githubusercontent.com/85706972/171280361-311d8e7c-c564-40aa-b960-905b72df5e57.png)
    
# Mitigation
    
<li>Microsoft have released official patch to fix this issues - here is more info on that - https://www.bitdefender.com/blog/hotforsecurity/microsoft-june-patch-tuesday-fixes-follina-zero-day-vulnerability/</li>
    

# Reference
    
<li> https://www.huntress.com/blog/microsoft-office-remote-code-execution-follina-msdt-bug
<li> https://msrc-blog.microsoft.com/2022/05/30/guidance-for-cve-2022-30190-microsoft-support-diagnostic-tool-vulnerability/
<li> https://thesecmaster.com/how-to-fix-cve-2022-30190-a-zero-click-rce-vulnerability-in-msdt/
  

    

