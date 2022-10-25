# device-net-snmp
Provides a Net-SNMP appliance

This appliance does the following:

- All parameters passed to the device commands are syntax checked and canonicalised, with bash completion.
- Automatically configures net-snmp's configuration files before startup.
- Binds securely to the unix domain socket /run/net-snmp/snmpd.sock.
- Autostarts on server restart.
- Zero Trust configuration.

## before

- Deploy the device-net-snmp package.

```
[root@server ~]# dnf install device-net-snmp
```

## set snmp parameters

To set the contact and location, do the following:

```
[root@server ~]# device services snmp set contact=john@example.com location="The moon"
```

## enable

To switch the appliance on, run this.

```
[root@server ~]# device services snmp enable 
```

## disable

To switch the appliance off, run this.

```
[root@server ~]# device services snmp disable  
```


# device-net-snmp-ssh
Provides an SSH extension to a Net-SNMP appliance

This extension does the following:

- All parameters passed to the device commands are syntax checked and canonicalised, with bash completion.
- Automatically configures net-snmp's and sshd's configuration files before startup.
- Enables the "snmp" subsytem in sshd.
- Autostarts on server restart.
- Zero Trust configuration.

Users that are allowed access via SSH are added using the device-net-snmp-tsm extension.

## enable

To enable SSH support, run this.

```
[root@server ~]# device services snmp ssh enable
```

## disable

To disable SSH support, run this.

```
[root@server ~]# device services snmp ssh disable
```



# device-net-snmp-tls
Provides a TLS extension to a Net-SNMP appliance

This extension does the following:

- All parameters passed to the device commands are syntax checked and canonicalised, with bash completion.
- Automatically configures net-snmp's configuration files before startup.
- Automatically identifies the correct server certificate, certificate chain, root
  certificate and key to use from certificates and keys placed under /etc/pki/tls,
  and configures net-snmp instance to use this certificate and key.
- Autostarts on server restart.
- Zero Trust configuration.

Users that are allowed access via TLS are added using the device-net-snmp-tsm extension.

## before

- Deploy the device-net-snmp-tls package.

```
[root@server ~]# dnf install device-net-snmp-tls
```

- Place certificates in files beneath the directory /etc/pki/tls/certs with extension
  pem. Place keys in files beneath the directory /etc/pki/tls/private. All certificates
  and keys will be scanned, only file extensions are important.

```
[root@server ~]# ls -l /etc/pki/tls/certs /etc/pki/tls/private
/etc/pki/tls/certs:
total 48
drwxr-xr-x. 2 root root 4096 Oct  2 21:35 .
drwxr-xr-x. 5 root root  104 Sep 23 21:03 ..
lrwxrwxrwx. 1 root root   49 Jul 28 21:08 ca-bundle.crt -> /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
lrwxrwxrwx. 1 root root   55 Jul 28 21:08 ca-bundle.trust.crt -> /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
-rw-r--r--. 1 root root 2278 Aug 20 12:50 seawitch.example.com-sectigo-2023-hostCert.pem
-rw-r--r--. 1 root root 4136 Aug 20 12:51 seawitch.example.com-sectigo-2023-hostChain.pem

/etc/pki/tls/private:
total 12
-rw-------. 1 root root 1705 Aug 20 12:44 seawitch.example.com-sectigo-2023-hostKey.pem
```

## set tls

To expose net-snmp with the hostname "seawitch.example.com" over
port 10161 securely using UDPDTLS, run this. The certificate, chain and key will be found
based on the hostname provided and configured automatically.

```
[root@server ~]# device services snmp tls set tls-dns=seawitch.example.com
```

## enable

To enable TLS support, run this.

```
[root@server ~]# device services snmp tls enable
```

## disable

To disable TLS support, run this.

```
[root@server ~]# device services snmp tls disable
```


# device-net-snmp-tsm
Provides a TSM extension to a Net-SNMP appliance

This extension does the following:

- All parameters passed to the device commands are syntax checked and canonicalised, with bash completion.
- Automatically configures net-snmp's configuration files before startup.
- Allows the specification of the name of a user that will be allowed to access SNMP over SSH, or over DTLS with the addition of a certificate below.
- Automatically identifies the correct client certificate from certificates placed under /etc/pki/tls/certs, and maps this certificate to the given user.
- Autostarts on server restart.
- Zero Trust configuration.

## before

- Deploy the device-net-snmp-tsm package.

```
[root@server ~]# dnf install device-net-snmp-tsm
```

- Place certificates in files beneath the directory /etc/pki/tls/certs with extension
  pem. All certificates will be scanned, only file extensions are important.

```
[root@server ~]# ls -l /etc/pki/tls/certs
/etc/pki/tls/certs:
total 48
drwxr-xr-x. 2 root root 4096 Oct  2 21:35 .
drwxr-xr-x. 5 root root  104 Sep 23 21:03 ..
lrwxrwxrwx. 1 root root   49 Jul 28 21:08 ca-bundle.crt -> /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
lrwxrwxrwx. 1 root root   55 Jul 28 21:08 ca-bundle.trust.crt -> /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
-rw-r--r--. 1 root root 4893 Aug 20 12:50 monitor.example.com-sectigo-2023-hostCert.pem
-rw-r--r--. 1 root root 4603 Aug 20 12:51 monitor.example.com-sectigo-2023-hostChain.pem
-rw-r--r--. 1 root root 2278 Aug 20 12:50 seawitch.example.com-sectigo-2023-hostCert.pem
-rw-r--r--. 1 root root 4136 Aug 20 12:51 seawitch.example.com-sectigo-2023-hostChain.pem
```

# add TSM user

To enable access via SSH or DTLS to the user "monitor" who way present a certificate matching "monitor.example.com", run this

```
[root@server ~]# device services snmp tsm add name="monitor" tls-dns=monitor.example.com
```

# remove TSM user

To remove the entry above with name "monitor", run this.

```
[root@server ~]# device services snmp tsm remove monitor
```

# set parameters for existing user

To update the certificate being used with the name monitor, run this.

```
[root@server ~]# device services snmp tsm set monitor tls-dns=monitor.example.com
```


