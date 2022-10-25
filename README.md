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

To disable appliance support, run this.

```
[root@server ~]# device services snmp ssh disable
```


