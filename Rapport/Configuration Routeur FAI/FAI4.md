en
conf t
hostname FAI4
interface gi0/0 
ip address 192.168.1.37 255.255.255.252
no shutdown
interface serial0/0/0
ip address 192.168.1.14 255.255.255.252
no shutdown
interface serial0/1/0
ip address 192.168.1.26 255.255.255.252
no shutdown
interface serial0/0/1
ip address 192.168.1.34 255.255.255.252
no shutdown
interface gi0/1
ip address 192.168.1.41 255.255.255.252
no shutdown

router rip
version 2
no auto-summary 
network 192.168.1.0 

enable secret admin_fai4

line console 0
password fai4
login
exit

ip domain-name "berthod" 
cryto key generate rsa
2048
ip ssh version 2 
username admin privilege 15 password vty_fai4
line vty 0 15
transport input telnet ssh  
login local
exit