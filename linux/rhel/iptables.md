iptables uses three different chains: input, forward, and output.
Input – This chain is used to control the behavior for incoming connections.
Forward – This chain data is just forwarded.
Output – This chain is used for outgoing connections.
RH-Firewall-1-INPUT – This is a user-defined custom chain. It is used by the INPUT, OUTPUT and FORWARD chains.


Connection-specific Responses
Accept – Allow the connection.
Drop – Drop the connection, if you don’t want the source to realize your system exists.
Reject – Don’t allow the connection, but send back an error.

iptables -A to append rules to the existing chain
iptables -I [chain] [number]  to insert ther rule.

List the currently configured iptables rules:
iptables -L
iptables --line-numbers -n -L

Edit /etc/sysconfig/iptables
Find lines:
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]


Update as follows to change the default policy to DROP from ACCEPT for the INPUT and FORWARD built-in chains:
:INPUT DROP [0:0]
:FORWARD DROP [0:0]

Log And Drop All Traffic
Find the lines:  -A RH-Firewall-1-INPUT -j REJECT --reject-with icmp-host-prohibited
Update it as follows:
-A RH-Firewall-1-INPUT -j LOG
-A RH-Firewall-1-INPUT -j DROP

To open port 80
-A RH-Firewall-1-INPUT -m tcp -p tcp --dport 80 -j ACCEPT

To open port 53 (DNS Server)
-A RH-Firewall-1-INPUT -m tcp -p tcp --dport 53 -j ACCEPT
-A RH-Firewall-1-INPUT -m udp -p tcp --dport 53 -j ACCEPT

Only allow SSH traffic From 192.168.1.0/24
-A RH-Firewall-1-INPUT -s 192.168.1.0/24 -m state --state NEW -p tcp --dport 22 -j ACCEPT

Enable Printing Access For 192.168.1.0/24
-A RH-Firewall-1-INPUT -s 192.168.1.0/24 -p udp -m udp --dport 631 -j ACCEPT
-A RH-Firewall-1-INPUT -s 192.168.1.0/24 -p tcp -m tcp --dport 631 -j ACCEPT

Open FTP Port 21
-A RH-Firewall-1-INPUT -m state --state NEW -p tcp --dport 21 -j ACCEPT



Log and Drop Spoofing Source Addresses

Append the following lines before final COMMIT line:
-A INPUT -i eth0 -s 10.0.0.0/8 -j LOG --log-prefix "IP DROP SPOOF "
-A INPUT -i eth0 -s 172.16.0.0/12 -j LOG --log-prefix "IP DROP SPOOF "
-A INPUT -i eth0 -s 192.168.0.0/16 -j LOG --log-prefix "IP DROP SPOOF "
-A INPUT -i eth0 -s 224.0.0.0/4 -j LOG --log-prefix "IP DROP MULTICAST "
-A INPUT -i eth0 -s 240.0.0.0/5 -j LOG --log-prefix "IP DROP SPOOF "
-A INPUT -i eth0 -d 127.0.0.0/8 -j LOG --log-prefix "IP DROP LOOPBACK "
-A INPUT -i eth0 -s 169.254.0.0/16  -j LOG --log-prefix "IP DROP MULTICAST "
-A INPUT -i eth0 -s 0.0.0.0/8  -j LOG --log-prefix "IP DROP "
-A INPUT -i eth0 -s  240.0.0.0/4  -j LOG --log-prefix "IP DROP "
-A INPUT -i eth0 -s  255.255.255.255/32  -j LOG --log-prefix "IP DROP  "
-A INPUT -i eth0 -s 168.254.0.0/16  -j LOG --log-prefix "IP DROP "
-A INPUT -i eth0 -s 248.0.0.0/5  -j LOG --log-prefix "IP DROP "





This example shows how to block all connections from the IP address 10.10.10.10
iptables -A INPUT -s 10.10.10.10 -j DROP

Connections from a range of IP addresses
  iptables -A INPUT -s 10.10.10.0/24 -j DROP
  iptables -A INPUT -s 10.10.10.0/255.255.255.0 -j DROP

Connections to a specific port
This example shows how to block SSH connections from 10.10.10.10.
iptables -A INPUT -p tcp --dport ssh -s 10.10.10.10 -j DROP

Connection States
example, where SSH connections FROM 10.10.10.10 are permitted, but SSH connections TO 10.10.10.10 are not. 

iptables -A INPUT -p tcp --dport ssh -s 10.10.10.10 -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -p tcp --sport 22 -d 10.10.10.10 -m state --state ESTABLISHED -j ACCEPT

sudo /sbin/iptables-save
    /sbin/service iptables save


To clear all the currently configured rules
    iptables -F


To start/stop/restart/reload the iptables on CentOS 7 / RHEL 7
Install iptables-services
systemctl start iptables

Manage iptables with systemctl
systemctl start iptables
service iptables [stop|start|restart|reload]


types of tables.

1. Filter table
2. NAT table
3. Mangle table
4. Raw table
5. Security table

Filter table is the default iptable decides if a packet should be allowed to its destination or not.

NAT  table contains the following chains.
1. PREROUTING chain – This chain is mainly for DNAT (Destination NAT)
2. POSTROUTING chain – This chain is mainly for SNAT (Source NAT)

view the NAT table using the following command.
sudo iptables -t filter --list

Mangle table
This table is primarily used for altering the IP headers. It has the following chains.

1. PREROUTING
2. OUTPUT
3. FORWARD
4. INPUT
5. POSTROUTING

View the mangle table list using the following command.

sudo iptables -t mangle --list

Raw table

This table provides a mechanism to mark packets for opting out of connection tracking
table has the following chains

1. PREROUTING
2. OUTPUT chain

sudo iptables -t raw --list

Security table

This table is related SELINUX. It sets SELINUX context on packets. To be more specific, it is used for Mandatory Access Control 
sudo iptables -t security --list
