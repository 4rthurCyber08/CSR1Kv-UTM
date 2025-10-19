~~~
!@UTM-PH
config t
 hostname UTM-PH
 enable secret pass
 service password-encryption
 no logging cons
 line vty 0 14
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
