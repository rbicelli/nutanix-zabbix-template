# Zabbix_Nutanix_Template
Template to monitor a Nutanix Cluster with Zabbix through SNMP v3

## Configuring the Template

### On Nutanix

Go to PRISM Console, navigate to Settings -> SNMP.

- Create a UDP transport, if doesn't exists
- Create a new user with priv type AES and auth type SHA and set passphrases
- Download MIB via "View MIB" button and save it as NUTANIX-MIB.txt

### In Zabbix

On Zabbix Server and Zabbix Proxy, if you are monitoring PRISM with a Proxy, copy the previously downloaded NUTANIX-MIB.txt file to /usr/share/snmp/mibs.
Then restart zabbix-server and/or zabbix-proxy services.

You may want to check if SNMP is working, so on Zabbix Server or Zabbix Proxy try with this command:

```
snmpwalk -v 3 -a SHA -A YOURSHAPASSWORD -u zabbix -x AES -X YOURAESPASSWORD IP_OF_PRISM_CENTRAL -l AuthPriv citContainerName
```

Debian users: if you encouter MIB parsing errors, you may have to install the package snmp-mibs-downloader package, which is available only enabling contrib and non-free repositories. See https://wiki.debian.org/DebianRepository

Import the template file provided.

Create new host with interface SNMP:
- SNMP version: SNMP v3
- Context name: leave empty
- Security name: username created in Nutanix Prism
- Security level: authPriv
- Authentication protocol: SHA1
- Autentication passphrase: SHA passphrase previously set in Nutanix Prism
- Privacy Protocol: AES128
- Privacy passphrase: AES passphrase previously set in Nutanix prism
 
Link the host to the template.

## Credits

How to use this template : 

  (FR) https://blog.devarieux.net/2016/03/template_nutanix_pour_zabbix.html
  (EN) https://blog.devarieux.net/2016/03/nutanix-template-for-zabbix.html

