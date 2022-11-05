## Networking

Check routing table information

```
$ route 
$ ip route
```

Add a network to current route

```
 ip route add <network_ip>/<cidr> via <gateway_ip> dev <network_card_name>
 
```
### Example
```
$ ip route add 192.168.10.0/24 via 10.175.3.1 dev eth1
```

```
$ route add -net 192.168.10.0 netmask 255.255.255.0 gw 10.175.3.1
```
## Footprinting & Scanning

Find live hosts with fping or nmap

```
$ fping -a -g 172.16.100.40/24 2>/dev/null | tee alive_hosts.txt
```

```
$ nmap -sn 172.16.100.40/24 -oN alive_hosts.txt

```


### nmap scan types
```
-sS: TCP SYN Scan (aka Stealth Scan)
-sT: TCP Connect Scan 
-sU: UDP Scan
-sn: Not Port Scan
-sV: Service Version information
-O: Operating System information
```

```
nmap --script=mysql-*
nmap -p27017 --script=mongodb-brute target-2
```

## Subdomain Enumeration

- [Sublist3r](https://github.com/aboul3la/Sublist3r)
- [DNSdumpster](https://dnsdumpster.com/)


## Web Server Fingerprinting

Use netcat for HTTP banner grabbing:
```
$ nc <target addr> 80
HEAD / HTTP/1.0
```

Use OpenSSL for HTTPS banner grabbing:
```
$ openssl s_client -connect target.site:443
HEAD / HTTP/1.0
```

httprint is a web fingerprinting tool that uses **signature-based** technique
to identify web servers. This is more accurate since sysadmins can customize
web server banners.

```
$ httprint -P0 -h <target hosts> -s <signature file>
```

## Google Hacking

site:
intitle:
inurl:
filetype:
AND, OR, &, |

```
Example : inurl:admin intitle:login
```
[Google Hacking Database ](https://www.exploit-db.com/google-hacking-database)

## XXS (Cross Site  Scripting)

Look to exploit user input coming from:
- Request headers
- Cookies
- Form inputs
- POST parameters
- GET parameters

Check for XSS
```
<script>alert(1)</script>
<i>some text</i>
```

Steal cookies:
```
<script>alert(document.cookie)</script>
```

### Xsser Tool 

```
xsser --url 'http://demo.ine.local/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS'

target_host=XSS > Is Variable

```

 ## XSSer's "--auto" option

 ```
 xsser --url 'http://demo.ine.local/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS' --auto

 ```
## Using custom XSS payload

 ```
xsser --url 'http://demo.ine.local/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS' --Fp "<script>alert(1)</script>"

 ```

 In Burp Suite, replace the POST parameters with the final attack 

```
target_host=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&dns-lookup-php-submit-button=Lookup+DNS

```
## Enum4linux 
- enum4linux -U # enum Users
- enum4linux -d -S # detail and Share list
- enum4linux -n # nbstat if server avaible you got <20>
- enum4linux -s ~/Desktop/wordlists/100-common-passwords.txt # share brute force


# Meterpreter Rooting
## pivot to the otherwise unreachanble network
```
run autoroute -s 192.69.228.0 -n 255.255.255.0
``````
> background
> 
> route print

```
route add 192.69.228.0 255.255.255.0 1
```
## Scan subnet with msf
```
use auxiliary/scanner/portscan/tcp
set PORTS 80, 8080, 445, 21, 22
set RHOSTS 192.69.228.3-10
exploit
```
## Forwarding the remote port to local port
```
portfwd add -l 1234 -p 21 -r 192.69.228.3
portfwd list
```
```
background
nmap -sS -sV -p 1234 localhost
```
# Meterpreter Post-Exploitation
## Enum_users_Information

```
use post/linux/gather/enum_users_history
```
## Setting Proxy Socks

```
use auxiliary/server/socks_proxy
```
# Meterpreter mysql
## remote server 
```
exploit/multi/mysql/mysql_udf_payload
```
# Find flag
```
find / -name flag.txt 2>/dev/null
find / -name user.txt 2>/dev/null
find / -name .flag 2>/dev/null
find / -name flag 2>/dev/null
find / -name root.txt 2>/dev/null
```

# Mysql 

show databases;
use gallery_db;
show tables;
select * from users;