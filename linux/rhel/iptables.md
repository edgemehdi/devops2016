iptables uses three different chains: input, forward, and output.
Input – This chain is used to control the behavior for incoming connections.
Forward – This chain data is just forwarded.
Output – This chain is used for outgoing connections.


Connection-specific Responses
Accept – Allow the connection.
Drop – Drop the connection, if you don’t want the source to realize your system exists.
Reject – Don’t allow the connection, but send back an error.

iptables -A to append rules to the existing chain
iptables -I [chain] [number]  to insert ther rule.

List the currently configured iptables rules:
iptables -L




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
