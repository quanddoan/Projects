# Configure Ubuntu to join Windows AD domain
1. First, we need to make sure that Ubuntu has access to the Internet through NAT network and our DHCP, DNS services
- Check through Settings or CLI that Ubuntu uses DHCP and has IP address within the range that we set up and DNS server is our Windows Server

2. Install required packages
```sudo apt install realmd adcli krb5-user samba-common-bin```

3. Set hostname
```sudo hostnamectl set-hostname <hostname.domain.suffix>```

4. Make sure that Ubuntu is time-synced with the server through NTP
- Use command ```sudo apt-get install ntp``` to install NTP
- Modify ```ntp.conf``` file using command ```sudo vi /etc/ntp.conf```
- Append ```server <domain.suffix>``` at the end of the file, where ```<domain.suffix>``` is the name of the domain. e.g. ```server rainforest.com```
- Restart NTP: ```sudo /etc/init.d/ntp restart```

5. Update ```krb5.conf``` file
- Use command ```sudo vi /etc/krb5.conf```
- Under ```[libdefaults]```, change ```default_domain = domain.net``` to your domain, and ```rdns = false```

6. Verify that Domain Controller is reachable
```sudo realmd discover <domain.suffix>```

7. Grab a Kerberos ticket
```sudo kinit <user>@<domain.suffix>``` where ```<user>``` is the logon name of a user in domain

8. Join domain:
```sudo realm join -v –membership-software=samba -U <user> <domain.suffix>```
- There will be a message to notify that machine has joined realm successfully

9. Have home directory for new AD user created automatically during first logon
```sudo pam-auth-update --enable mkhomedir```

10. Verify that the AD user account is now set up in the machine
```check <user>@<domain.suffix>```



