# Exercises 3

<br>

## A Priori

### Installing...

```shell
sudo - root
```

#### Desativar Firewall

```shell
systemctl stop iptables
```

<br>

```shell
yum install epel-release
```

<br>

## OpenVPN installation in Linux

### 1. Install OpenVPN in Linux (available at https://openvpn.net)

```shell
yum install openvpn
```

<br>

### 2. Configure OpenVPN as a Linux service and to start at system boot

#### Mudar de Diretorio

```shell
cd /usr/share/doc/openvpn-2.4.12/sample/sample-config-files/
```

#### Copiar

```shell
cp client.conf /etc/openvpn/client/
```

#### Mudar de Diretorio

```shell
cd /etc/openvpn/client
```

#### Configurar

```shell
nano client.conf
```

```text
ca ca.crt
cert client.crt
key client.key
```

* NOTA: Os seguintes ficheiros/modificações tem que ser os mesmos tanto no servidor como no cliente:

```text
# Diffie hellman parameters.
# Generate your own with:
#   openssl dhparam -out dh2048.pem 2048
dh dh2048.pem
```

```text
# For extra security beyond that provided
# by SSL/TLS, create an "HMAC firewall"
# to help block DoS attacks and UDP port flooding.
#
# Generate with:
#   openvpn --genkey --secret ta.key
#
# The server and each client must have
# a copy of this key.
# The second parameter should be '0'
# on the server and '1' on the clients.
tls-auth ta.key 0 # This file is secret
```

#### Conectar 

```shell
openvpn /etc/openvpn/client/client.conf
```

* NOTA: Para o servidor é identico, trocar `client` por `server` e fazer os procedimentos acima:

#### Exemplos:

```shell
nano server.conf
```

```shell
openvpn /etc/openvpn/server/server.conf
```


#### Check

```shell
ping <ip>
```

```shell
route -n
```

#### 

```shell

```

<br>

### 



