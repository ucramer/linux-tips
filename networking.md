# Linux Networking
Some things I learned on different Linux systems that I have to do now and again on the network side of things.

## Disclaimer:
These commands impact the availability of the system. Although I am trying to ensure that everything works, there could be changes/differences in systems that impact how the command is implemented. 
Using these commands are at the users own risk and care should be taken.

## 1. Listening Ports
Listening ports are open ports where a service is listening on to accept a connection. Finding out which ports are listening can assist in troubleshooting services that don't start. 
Or prevent duplications. Especially if you run Docker and have multiple containers providing web services. 
For Debian, I normally use `ss` as follows:
```console
root@vm:~# ss -tulpn | grep LISTEN
tcp   LISTEN 0      128          0.0.0.0:22         0.0.0.0:*    users:(("sshd",pid=1109,fd=6))
tcp   LISTEN 0      128             [::]:22            [::]:*    users:(("sshd",pid=1109,fd=7))
```
In teh above output, the SSH Daemon (sshd) is listening on all connections (IPv4: 0.0.0.0 / IPv6 [::]) on port 22. It also shows the Processor ID (pid=1109).

## 2. Chaning IP Addresses:
*(to be completed)*

## 3. Displaying the IP on the CLI Login Screen
Modern hypervisors have tools (VMware Tools for VMware, virtuo for KVM-based hypervisors) that can extract that information and display it in the VM information. 
With VirtualBox, identifying the IP would require commmands on the Command Prompt (Windows) to find the IP. Having the IP displayed on the Login screen, would allow you to open the console and see the IP there. 

### Steps:
1. Log into the machine.
2. Identify the adapter name for which you want to display the IP :
```console
user@vm:~ $ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 88:a2:9e:a7:fd:9b brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.50/24 brd 192.168.10.255 scope global noprefixroute eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::8aa2:9eff:fea7:fd9b/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
We want to use `eth0`.
3. Edit the `/etc/issue` file. This requires elevated priviledges:
```console
user@vm:~ $ sudo nano /etc/issue
[sudo] password for user:
```
4. add the following line:
```
IP Address: \4{eth0}
```
5. Save and exit.
6. Your output on the login screen should look something like this:
```
Debian GNU/Linux 13 VM tty1

IP Address: 192.168.10.50
```
