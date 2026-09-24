en
conf t
hostname FAI2
interface gi0/0 
ip address 192.168.1.17 255.255.255.252
no shutdown
interface serial0/0/1
ip address 192.168.1.45  255.255.255.252
no shutdown
interface serial0/0/0
ip address 192.168.1.21 255.255.255.252
no shutdown
interface serial0/1/0
ip address 192.168.1.25  255.255.255.252
no shutdown

router rip
no auto-summary
version 2
network 192.168.1.0 

enable secret admin_fai2

line console 0
password fai2
login
exit

ip domain-name "berthod" 
cryto key generate rsa
2048
ip ssh version 2 
username admin privilege 15 password vty_fai2
line vty 0 15
transport input telnet ssh  
login local
exit
