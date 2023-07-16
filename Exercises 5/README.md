# Practical Exercises #5 - Configure network packet filtering and NAT using IPTables

## Configure a Linux system to operate as a router (by enabling packet forwarding) between two IPv4 networks: 10.254.0.0/24 (representing the internal network) and 172.16.1.0/24 (the external network).

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

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

## Clear your IPTables (firewall) configuration

```bash
iptables -F
```


<br>

## Create a network firewall configuration to implement the following security policy:

- Lista dos Portos dos Serviços:

```bash
more /etc/services
```

```bash
cat /etc/services | grep domain
```

- Forward:

```bash
iptables -P FORWARD
```


### Authorize the following communications between the two networks (direct IP communications, therefore without NAT):

#### DNS queries from hosts on the internal network to DNS servers on the external network.

```bash
iptables -A FORWARD -s 10.60.0.0/24 -d 193.136.212.0/24 -p udp --dport domain -j ACCEPT
```

```bash
iptables -A FORWARD -s 193.136.212.0/24 -d 10.60.0.0/24  -p udp --sport domain -j ACCEPT
```

#### Network time synchronization requests from hosts on the internal network to NTP servers on the external network.


```bash
iptables -A FORWARD -s 10.60.0.0/24 -d 193.136.212.0/24 -p udp --dport ntp -j ACCEPT
```

```bash
iptables -A FORWARD -s 193.136.212.0/24 -d 10.60.0.0/24  -p udp --sport ntp -j ACCEPT
```

```bash
iptables -L
```

```bash
iptables -L -v
```

### Authorize the following communications between the two networks using SNAT (Source NAT):

#### SSH, HTTP and HTTPS connections from hosts on the internal network to servers on the external network.

```bash
iptables -A FORWARD -s 10.60.0.0/24 -d 193.136.212.0/24  -p tcp --dport ssh -j ACCEPT
```

```bash
iptables -A FORWARD -s 193.136.212.0/24 -d 10.60.0.0/24  -p tcp --sport ssh -j ACCEPT
```

```bash
iptables -t nat -A POSTROUTING -s 10.60.0.0/24 -d 193.136.212.0/24  -p tcp --dport ssh -j SNAT --to-source 193.136.212.2
```

```bash
iptables -t nat -A PREROUTING -s 193.136.212.0/24 -d 10.60.0.0/24  -p tcp --dport ssh -j DNAT --to-destination 10.60.0.2
```

#### FTP connections from hosts on the internal network to a server on the external network (in passive and active modes).

```bash
yum install vsftpd
```

- Listar todos os serviços ativos em X portos

```bash
netstat -lnp 
```


