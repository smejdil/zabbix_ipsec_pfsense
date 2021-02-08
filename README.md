# Monitoring IPsec tunnels on pfSense using zabbix

This project was forked from https://github.com/jpmenil/zabbix-templates, https://github.com/alanwds/zabbix_ipsec_pfsense and https://github.com/fralvarezcalvo/zabbix_ipsec_pfsense.

Thanks to @alanwds, @jpmenil and @fralvarezcalvo by share

This template is used for monitoring IPSEC tunnels on PFSense using zabbix.

# Dependencies

- Project is used for pfSense >= 2.4.5
- Zabbix agent (you can install it from pfsense packages manager)
- sudo (you can install it from pfsense packages manager)
- Zabbix Server >= 4.0
- check_ipsec.sh
- check_ipsec_traffic.sh
- zabbix-ipsec.py (use Python 3.7)
- zabbix_sudoers

# How it works

The template queries zabbix-ipsec.py for tunnels ids (conXXXX). After that, the items prototipes are created consuming check_ipsec.sh script. The script check_ipsec_traffic is used to collect traffic about the tunnel.

### Installation

- You have to put check_ipsec.sh, check_ipsec_traffic.sh and zabbix-ipsec.py on pfsense filesystem. (/usr/local/bin/ in this example)
- Install sudo pakage at pfsense packages manager
- Copy file zabbix_sudoers under /usr/local/etc/sudoers.d
- Enabled Custom Configuration on Advanced Settins at System -> sudo
- Create the follow user parameters at zabbix-agent config page on pfsense (Service -> Zabbix-agent -> Advanced Options)
```
UserParameter=ipsec.discover,/usr/local/bin/python3.7 /usr/local/bin/zabbix-ipsec.py
UserParameter=ipsec.tunnel[*],/usr/local/bin/sudo /usr/local/bin/check_ipsec.sh $1
UserParameter=ipsec.traffic[*],/usr/local/bin/sudo /usr/local/bin/check_ipsec_traffic.sh $1 $2
```
- Set execution permissions
```
chmod +x /usr/local/bin/zabbix-ipsec.py
chmod +x /usr/local/bin/check_ipsec.sh 
chmod +x /usr/local/bin/check_ipsec_traffic.sh 
``` 
- Import the template ipsec_template-X.xml on zabbix and attach to pfsense hosts
- Go get a beer