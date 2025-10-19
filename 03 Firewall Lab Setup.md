
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

### 10. Configure CSR1000v
When CSR1000v finish loading, it typically has a lot of logs on screen, make sure to enter the VM then enter `enable` __[17]__ to gain access to privilege mode.  

Now simply copy the following script:
~~~
!@UTM-1
config t
 hostname UTM-1
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

<br>

Then, back to VMWare, select `Edit` __[18]__ > `Paste` __[19]__

<br>

![10-FW](<img/00 CSR-10.png>)

&nbsp;
---
&nbsp;

### 11. Verify IP Address
Check the IP address of UTM-1 on its GigabitEthernet2 interface. It should be `192.168.102.11` __[20]__

> [!NOTE]
> If the status of the interface is down, simply enter the `show ip int brief` command again.

<br>

![11-FW](<img/00 CSR-11.png>)

<br>

On CMD, ping the IP address `192.168.102.11` __[21]__ to see if it's replying. 

<br>

![12-FW](<img/00 CSR-12.png>)

<br>

Also, ping the following domain names, both should reply.
- `www.web310.com` __[22]__
- `www.web311.com` __[23]__

<br>

![13-FW](<img/00 CSR-13.png>)

&nbsp;
---
&nbsp;

### 12. Verification
Verify the open ports of both websites through nmap.
~~~
!@cmd
nmap -v www.web310.com
nmap -v www.web311.com
~~~

<br>

![14-FW](<img/00 CSR-14.png>)

&nbsp;
---
&nbsp;

### 13. Remote Access UTM-1
Go to SecureCRT then `telnet` __[24]__ `192.168.102.11` __[25]__. Then, `Connect` __[26]__

<br>

![15-FW](<img/00 CSR-15.png>)

&nbsp;
---
&nbsp;

### 14. Verify Internet access
Ping `8.8.8.8` __[27]__  
Ping `www.rivanit.com` __[28]__

<br>

Both should be pingable

<br>

![16-FW](<img/00 CSR-16.png>)

&nbsp;
---
&nbsp;

### 15. Access the GUI
On a browser, access the IP:
`https://192.168.102.11/` __[29]__  

<br>

Then, simply ignore the warning, select `Advance...` __[30]__ > `Accept the Risk and Continue` __[31]__

<br>

![17-FW](<img/00 CSR-17.png>)

&nbsp;
---
&nbsp;

### 16. Login to the GUI
Username: `admin` __[32]__
Password: `pass` __[33]__

<br>

Then, `Log In` __[34]__

<br>

![18-FW](<img/00 CSR-18.png>)

<br>
<br>

---
&nbsp;

### 🎯 Exercise 01 - Create an ACL policy named, ACL-POL1
Open the following ports:
| Web            | Ports           |
| ---            | ---             |
| www.web310.com | HTTPS, SSH, FTP |
| www.web311.com | PING, DNS       |

<br>

- Have a Sequence difference of `5`

&nbsp;
---
&nbsp;

### How to create a rule to open 443 for www.web310.com
On the CSR1000V dashboard, go to `Configuration` __[35]__ > `ACL` __[36]__

<br>

![19-FW](<img/00 CSR-19.png>)

&nbsp;
---
&nbsp;

On the ACL page, select `+ Add` __[37]__  

<br>

![20-FW](<img/00 CSR-20.png>)

&nbsp;
---
&nbsp;

Modify the ACL:
- ACL Name: `ACL-POL1` __[38]__
- ACL Type: `IPv4 Extended` __[39]__

<br>

__Rules__
- Sequence: 1
- Source Type: `any` __[41]__
- Destination Type: `Host` __[42]__
- Host Name*: `www.web310.com`
- Protocol: `TCP`  __[43]__
- Source Port: `None` __[44]__
- Destination Port: `eq` __[45]__
- Select Port*: `443` __[46]__

<br>

`+ Add` __[47]__ the rule to the ACL

<br>

![21-FW](<img/00 CSR-21.png>)

&nbsp;
---
&nbsp;

### How to allow ping for www.web311.com
Select the Destination Host as, `www.web311.com` __[49]__  
Then, add another rule, simply select the Protocol as `ICMP` __[48]__.

<br>

Then simply `+ Add`

![22-FW](<img/00 CSR-22.png>)

&nbsp;
---
&nbsp;

### Expected output
Once you configured the necessary rules the output should look as follows.

<br>

Once completed, `Apply to Device` __[50]__

![23-FW](<img/00 CSR-23.png>)

&nbsp;
---
&nbsp;

### How to assign the ACL to an interface
Once the ACL has been created, select `Associate Interfaces` __[51]__

<br>

![24-FW](<img/00 CSR-24.png>)

&nbsp;
---
&nbsp;

### Associate Interfaces
We will attach this ACL to `GigabitEthernet3` __[52]__. Then, set the IPv4 Inbound ACL `ACL-POL1` __[53]__.

<br>

Finally, `Apply to Device` __[54]__

<br>

![25-FW](<img/00 CSR-25.png>)

### Verify the Configuration
NMAP & Ping both websites to see if the configurations are correct.


<br>

![26-FW](<img/00 CSR-26.png>)
