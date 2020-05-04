### unifi-to-hosts-mapping

Note: Updated to support pyunifi library as unifi library is unsupported.

Within the UniFi controller you can create an alias for devices as they appear on the network. This script takes the alias entries and their corresponding IP address to maintain a list of hosts on the PiHole server. This host entry resides in /etc/hosts which the PiHole can be configured to use. The end result is the dashboard showing your configured hostname for Top Clients and not just an IP Address. 

Place script on PiHole server, in my case this is in /home/pi. You will also need to ensure you have the relevant python libraries installed. 

```
sudo pip install python_hosts
sudo pip install pyunifi
sudo pip install netaddr

```
The script requires the controller IP (or hostname), controller username and password to function correctly.  You can specify each of these via a command line parameter (`-c`, `-u` and `-p`), an environment variable (`UNIFI_CONTROLLER`, `UNIFI_USER` and `UNIFI_PASSWORD`), or interactively (in that order of precedence). 

Still in the works (in the mean time hard coded to ssl_verify=False): 
It will also support verification of an SSL Certificate using command line parameter ('-s') or an environment variable ('SSL_CERT_VAR'), with options True, False or defining path to specific certificate "path/to/custom_cert.pem". I have defaulted it to False which is opposite to pyunifi library.

Create a suitable crontab entry for the root user, it will need to be for the root user as you are modifying the /etc/hosts file. 

```
sudo crontab -e 

Add the following lines to the cron file and save/exit

0,15,30,45 * * * * /home/pi/client-mapping.py
```
If not using environment variables, use:
```
sudo crontab -e 

Add the following lines to the cron file and save/exit

0,15,30,45 * * * * /home/pi/client-mapping.py -c [Controller IP] -u [Controller username] -p [Controller password]
```


Finally, ensure you have selected "Reverse DNS lookup" for "Top Clients" within Settings on the PiHole Server 


