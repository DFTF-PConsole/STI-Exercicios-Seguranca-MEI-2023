# Exercises 2

<br>

## A Priori

### Installing...

```shell
sudo - root
```

```shell
yum install httpd mod_ssl
```

<br>

## Create a private Certification Authority (CA)

### 1. Create a private CA using OpenSSL in Linux

```shell
cd /etc/pki/CA/
```

#### Private Key (ca.key)

```shell
openssl genrsa -out ca.key 1024 -des3
```

#### Signing Req (ca.csr)

```shell
openssl req -new -key ca.key -out ca.csr
```

```text
	PT
	Coimbra
	Coimbra
	UC
	DEI
	STI_CA
	dario@student.dei.uc.pt
	
```

#### Certificate (ca.crt)

```shell
openssl x509 -req -days 365 -in ca.csr -out ca.crt -signkey ca.key
```

#### Ver o Certificado:

```shell
openssl x509 -in ca.crt -text
```

<br>

### 2. Create a X.509 certificate for the Apache server using the new CA

#### Private Key (ca.key)

```shell
openssl genrsa -out apache.key 1024 -des3
```

#### Signing Req (ca.csr)

```shell
openssl req -new -key apache.key -out apache.csr
```

* NOTA: Common Name: tem que ser hostname/website

```text
	PT
	Coimbra
	Coimbra
	UC
	DEI
	www.uc.pt
	dario@student.dei.uc.pt
	
```

#### Certificate (ca.crt)

##### A priori

```shell
touch /etc/pki/CA/index.txt
```

```shell
echo 01 > serial
```

##### Criar

```shell
openssl ca -in apache.csr -cert ca.crt -keyfile ca.key -out apache.crt
```

```shell
	y
	y
```

##### Ver Certificado

```shell
openssl x509 -in apache.crt -text
```

<br>

## Configure Apache with server authentication

### 3. Configure Apache to use the previously created X.509 certificate


```shell
nano /etc/httpd/conf.d/ssl.conf
```

```text
	SSLCertificateFile /etc/pki/CA/apache.crt
	SSLCertificateKeyFile /etc/pki/CA/apache.key
	SSLCACertificateFile  /etc/pki/CA/ca.crt
```

```shell
systemctl restart httpd
```

```shell
gedit /etc/hosts
```

### 4. Connect to the server (the new CA isn’t recognized yet)

```shell

```

### 5. Install the CA on the browser and repeat the previous test

```shell

```

<br>

## Configure Apache with client authentication

### 6. Create a personal X.509 certificate using the new CA

```shell

```

### 7. Configure Apache to require client authentication using X.509 certificates

```shell

```

### 8. Connect to the Apache server without using your personal certificate (the connection should be refused, check the server’s logs)

```shell

```

### 9. Install the personal certificate on the browser and repeat the previous test

```shell

```


