# Practical Exercises #4 - Use IPTables to configure a Linux system firewall

## Disabling Firewalld and enabling IPTables on CentOS 7

```bash
yum install iptables-services
```

```bash
systemctl stop firewalld
```

```bash
systemctl disable firewalld
```

```bash
systemctl mask firewalld
```

```bash
systemctl enable iptables
```

<br>

## 1. Clear all the rules on the system’s firewall configuration

```bash
systemctl stop iptables 
```

### OU...

```bash
iptables -F
```

<br>

### List all rules in the selected chain

```bash
iptables -L
```

<br>

## 2. Create a firewall rule to ignore incoming ping requests from another host (can be tested using the localhost address – 127.0.0.1), while authorizing all the remaining IP packets. Note: ping uses ICMP packets of types 8 (echo request) and 0 (echo reply)

```bash
iptables -A INPUT -s 127.0.0.1 -p icmp --icmp-type echo-request -j DROP
```

<br>

### Verbose output

```bash
iptables -L -v
```

<br>

## 3. Create firewall rules to authorize the following incoming TCP connections (filter table, INPUT chain), while rejecting (only) other TCP communications:

### a. SSH connections originated at the server student.dei.uc.pt

```bash
iptables -A INPUT -s student.dei.uc.pt -p tcp --dport ssh -j ACCEPT
```

<br>

### b. POP3 and IMAP4 connections originated at any other hosts.

```bash
iptables -A INPUT -p tcp --dport pop3 -j ACCEPT
```

```bash
iptables -A INPUT -p tcp --dport imap -j ACCEPT
```

<br>

```bash
iptables -A INPUT -p tcp --syn -j DROP
```

<br>

## 4. Add to the previous configuration firewall rules to authorize the following outgoing TCP connections (filter table, OUTPUT chain), while rejecting (only) other TCP communications:

### a. HTTP and HTTPS connections destined to the server student.dei.uc.pt

```bash
iptables -A OUTPUT -d student.dei.uc.pt -p tcp --dport https -j ACCEPT
```

```bash
iptables -A OUTPUT -d student.dei.uc.pt -p tcp --dport http -j ACCEPT
```

<br>

### b. SSH connections destined to any other hosts.

```bash
iptables -A OUTPUT -p tcp --dport ssh -j ACCEPT
```

<br>

```bash
iptables -A OUTPUT -p tcp --syn -j DROP
```

<br>

## 5. Clear all the firewall rules defined in the previous exercises

```bash
iptables -F
```

<br>

## 6. Use IPTables to authorize the following communications, while denying the remaining IP traffic (policy DROP on both the INPUT and OUTPUT chains):

```bash
iptables -P INPUT DROP
```

```bash
iptables -P OUTPUT DROP
```

### a. Incoming SSH and HTTP connections

```bash
iptables -A INPUT -p tcp --dport ssh -j ACCEPT
```

```bash
iptables -A OUTPUT -p tcp --sport ssh -j ACCEPT
```

```bash
iptables -A INPUT -p tcp --dport http -j ACCEPT
```

```bash
iptables -A OUTPUT -p tcp --sport http -j ACCEPT
```

<br>

### b. Outgoing SSH, HTTP and HTTPS connections

```bash
iptables -A INPUT -p tcp --sport ssh -j ACCEPT
```

```bash
iptables -A OUTPUT -p tcp --dport ssh -j ACCEPT
```

```bash
iptables -A INPUT -p tcp --sport http -j ACCEPT
```

```bash
iptables -A OUTPUT -p tcp --dport http -j ACCEPT
```

```bash
iptables -A INPUT -p tcp --sport https -j ACCEPT
```

```bash
iptables -A OUTPUT -p tcp --dport https -j ACCEPT
```


<br>

### c. DNS queries sent to the server dns.dei.uc.pt and dns2.dei.uc.pt

```bash

```

<br>

### d. Incoming ping requests from the server student.dei.uc.pt

```bash

```

<br>

### e. All IP communications to or from the localhost (127.0.0.1, or interface lo)

```bash

```

<br>

## 7. Activate the previous firewall configuration permanently on the system

```bash

```

### Guardar configs num ficheiro

```bash
iptables-save > my_fw_config
```

```bash
iptables-restore < my_fw_config
```

<br>


