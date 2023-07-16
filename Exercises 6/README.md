# Practical Exercises #6 - Intrusion detection with Snort

## Snort in the packet sniffer and logging modes

### 1. Download and install the Snort intrusion detection system with support for the “nfq” DAQ.

#### Instalar 

```bash
...
```

<br>

#### Testar


```bash
/usr/sbin/snort -v
```

```bash
/usr/sbin/snort --daq-list
```

<br>

### 2. Use Snort as a packet sniffer to print all packets (TCP/IP headers and packet contents).

```bash
mkdir /var/log/snort
```

```bash
snort -vd -i lo 
```

```bash
ping 127.0.0.1
```

<br>

### 3. Use Snort as a packet logger to:

#### Log all communications (headers and packet contents) in ascii mode.

```bash
snort -K ascii -vd -i lo -l /var/log/snort
```

```bash
ping 127.0.0.1
```

<br>

#### Use wireshark (or tshark) to analyze the logs.

```bash
snort -vd -i lo -l /var/log/snort
```

- Depois importar no wireshark

<br>

## Snort as a network intrusion detection system

### 4. Build a configuration file with rules for Snort applicable to the following types of communications:

#### Log all ICMP packets detected

```bash
nano /var/log/snort/rules
```

```text
log icmp any any -> any any (sid:0001;)
alert tcp any any -> any 80 (sid:0002;msg:"Detection POST";content:"POST";nocase;)
```

```bash
snort -vd -i lo -l /var/log/snort -c /var/log/snort/rules -K ascii -k none
```

```bash
ping 127.0.0.1
```

```bash
nc 127.0.0.1 80
POST
```

<br>

#### Alert when “POST” commands are detected in HTTP connections.

```bash

```

<br>


### 5. Run Snort inline using the NFQ DAQ module. Configure Snort and IPTables to block all HTTP communications using the “GET” command.


```bash

```

<br>



