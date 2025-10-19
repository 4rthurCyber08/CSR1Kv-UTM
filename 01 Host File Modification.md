
<!-- Your monitor number = #$34T# -->

# 💡 Modify Windows Host File

### 1. Access Network Connections Control Panel
Press __[Win + R]__ to open Run window. Then type in the path for the host file `c:\Windows\System32\drivers\etc` __[01]__

<br>

![01-UTM](<img/00 UTM-01.png>)

&nbsp;
---
&nbsp;

### 2. Open the hosts file
> [!NOTE]
> The hosts file can only be modified with Administrative Privilege.

Right-click the __hosts__ file, then `Edit with Notepad++` __[02]__  

<br>

![02-UTM](<img/00 UTM-02.png>)

&nbsp;
---
&nbsp;

### 3. Add the following entries to the hosts file
Add the following lines:
- 192.168.103.10 www.web310.com
- 192.168.103.11 www.web310.com

> [!IMPORTANT]
> Make sure to SAVE the file

<br>

![02-UTM](<img/00 UTM-03.png>)
