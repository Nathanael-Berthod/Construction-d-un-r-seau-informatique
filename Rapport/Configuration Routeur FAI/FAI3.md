en
conf t
hostname FAI3
interface gi0/0 
ip address 192.168.1.29  255.255.255.252
no shutdown
interface serial0/1/0
ip address 192.168.1.10  255.255.255.252
no shutdown
interface serial0/0/1
ip address 192.168.1.22 255.255.255.252
no shutdown
interface serial0/0/0
ip address 192.168.1.33  255.255.255.252
no shutdown

router rip
no auto-summary 
version 2
network 192.168.1.0 

enable secret admin_fai3

line console 0
password fai3
login
exit

ip domain-name "berthod" 
cryto key generate rsa
2048
ip ssh version 2 
username admin privilege 15 password vty_fai3
line vty 0 15
transport input telnet ssh  
login local
exit