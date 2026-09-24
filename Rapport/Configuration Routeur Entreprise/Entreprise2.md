en 
conf t
hostname re2
interface gi0/1
ip address 192.168.1.18 255.255.255.252
no shutdown 
int gi0/0
no shutdown

interface gi0/0.10
encapsulation dot1q 10
ip address 172.16.68.97 255.255.255.240
interface gi0/0.20
encapsulation dot1q 20
ip address 172.16.68.65 255.255.255.224
interface gi0/0.30
encapsulation dot1q 30
ip address 172.16.64.1 255.255.254.0
interface gi0/0.40
encapsulation dot1q 40 
ip address 172.16.66.1 255.255.254.0
interface gi0/0.100
encapsulation dot1q 100
ip address 172.16.68.1  255.255.255.192

router rip 
version 2
no auto-summary
network 192.168.1.0 
network 172.16.0.0

enable secret admin_re2

line console 0
password re2
login
exit

ip domain-name "berthod" 
cryto key generate rsa
2048
ip ssh version 2 
username admin privilege 15 password vty_re2
line vty 0 15
transport input telnet ssh  
login local
exit
