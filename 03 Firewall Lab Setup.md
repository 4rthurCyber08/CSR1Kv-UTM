
<!-- Your monitor number = #$34T# -->

# 💡 Firewall Lab (Protect Ports)

### 1. CSR1000v
Open `CSR1000v` __[01]__

<br>

![01-FW](<img/00 CSR-01.png>)

&nbsp;
---
&nbsp;

### 2. Name
Set the name as `UTM-1` __[02]__. Then, select `Next` __[03]__

<br>

![02-FW](<img/00 CSR-02.png>)

&nbsp;
---
&nbsp;

### 3. Deployment Options
Leave as default, `small` __[04]__. Then `Next` __[05]__

<br>

![03-FW](<img/00 CSR-03.png>)

&nbsp;
---
&nbsp;

### 4. Properties
Leave as default, `import` __[06]__

<br>

![04-FW](<img/00 CSR-04.png>)

&nbsp;
---
&nbsp;

### 5. Modify CSR Network Adapters
Right-click the tab __UTM-1__, then select `Settings...` __[07]__ 

<br>

![05-FW](<img/00 CSR-05.png>)

&nbsp;
---
&nbsp;

### 6. NetAdapter 1
Select the first `Network Adapter` __[08]__, then set it to `NAT` __[09]__

<br>

![06-FW](<img/00 CSR-06.png>)

&nbsp;
---
&nbsp;

### 7. NetAdapter 2
Select `Network Adapter 2` __[10]__, then select `Custom` __[11]__ > `VMNet2 (Host-Only)` __[12]__

<br>

![07-FW](<img/00 CSR-07.png>)

&nbsp;
---
&nbsp;

### 8. NetAdapter 3
Select `Network Adapter 3` __[13]__, then select `Custom` __[14]__ > `VMNet3 (Host-Only)` __[15]__

<br>

![08-FW](<img/00 CSR-08.png>)

&nbsp;
---
&nbsp;

### 9. Confirm
Confirm the setup by selecting `OK` __[16]__
<br>

![09-FW](<img/00 CSR-09.png>)

&nbsp;
---
&nbsp;








~~~
!@UTM-PH
config t
 hostname UTM-PH
 enable secret pass
 service password-encryption
 no logging cons
 line vty 0 14
  transport input all
  password pass
  login local
  exec-timeout 0 0
 username admin privilege 15 secret pass
 int g1
  no shut
  ip add 208.8.8.11 255.255.255.0
 int g2
   no shut
   ip add 192.168.102.11 255.255.255.0
 int gi 3
  no shut
  ip add 192.168.103.11 255.255.255.0
  ip add 192.168.103.10 255.255.255.0 Secondary
 service finger
 service tcp-small-servers
 service udp-small-servers
 ip dns server
 ip http server
 ip http secure-server
 ip http authentication local
 ip host www.web310.com 192.168.103.10
 ip host www.web311.com 192.168.103.11
 ip name-server 8.8.8.8 1.1.1.1
 ip route 0.0.0.0 0.0.0.0 208.8.8.2
 ip domain lookup
 end
show ip int br
~~~



### Exercise 01 - Create an ACL policy named, ACL-POL1
Open the following ports:
| Web            | Ports           |
| ---            | ---             |
| www.web310.com | HTTPS, SSH, FTP |
| www.web311.com | PING, DNS       |

