en
conf t
hostname FAI1
interface gi0/0 
ip address 192.168.1.1 255.255.255.252
no shutdown
interface serial0/0/0
ip address 192.168.1.5  255.255.255.252
no shutdown
interface serial0/1/0
ip address 192.168.1.9  255.255.255.252
no shutdown
interface serial0/0/1
ip address 192.168.1.13  255.255.255.252
no shutdown

router rip
version 2
no auto-summary
network 192.168.1.0 

enable secret admin_fai1

line console 0
password fai1
login
exit

ip domain-name "berthod" 
cryto key generate rsa
2048
ip ssh version 2 
username admin privilege 15 password vty_fai1
line vty 0 15
transport input telnet ssh  
login local
exit



