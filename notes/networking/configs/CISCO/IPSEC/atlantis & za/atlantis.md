```
hostname Plato
!
no ip domain lookup
!
crypto isakmp policy 1
 encr aes
 authentication pre-share
 group 2
crypto isakmp key 6 CISCO address ****WAN PEER****
!
!
crypto ipsec transform-set ROUTERATLANTIS esp-aes esp-sha-hmac
!
crypto map ATLANTIS_ZA 10 ipsec-isakmp
 set peer 146.4.13.221****WAN PEER****
 set transform-set ROUTERATLANTIS
 match address 101
!
!
!
interface FastEthernet0/0
 ip address 192.168.3.254 255.255.255.0
 ip nat inside
!
interface FastEthernet0/1
 ip address dhcp
 ip nat outside
 crypto map ATLANTIS_ZA
!
ip nat inside source list NONAT interface FastEthernet0/1 overload
!
ip access-list extended NONAT
 deny   ip 192.168.3.0 0.0.0.255 192.168.4.0 0.0.0.255
 permit ip 192.168.3.0 0.0.0.255 any
!
access-list 101 permit ip 192.168.3.0 0.0.0.255 192.168.4.0 0.0.0.255
!
line con 0
 exec-timeout 0 0
 logging synchronous
!
end
```