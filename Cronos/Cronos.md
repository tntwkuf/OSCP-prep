Cronos is medium difficulty OSCP-like machine based on Linux.
Let's start our research with a nmap scan to detect open ports, running services and soft version:
![](1.png)
We can spot an 22, 53 and 80 open ports. The last one is a web-server:
![](2.png)
Fuzzing directories is don't give us interesting information except URL /web.config:
![](3.png)
Web.config's content is a rules of webserver that matches end replaces URL with regexp:
![](4.png)
Further directories fuzzing showed no results unlike subdomains fuzzing:
![](5.png)
There we can see a subdomain «admin». Let's check it out:
![](6.png)
Here we can spot an auth panel. Additional checks shows us an SQL-injection that we can use to bypass auth system:
![](7.png)
and we receive a redirect to /welcome.php:
![](8.png)
Let's see that page in a browser:
![](9.png)
It seems like a ping online tool which often vulnerable to command-injections. Let's try to execute any command by adding a semicolon:
![](10.png)
Now we can do many things such as reading local files with config:
![]11.png)
Next step is gaining reverse shell with python command and running netcat on our attacker machine:
![](12.png)
That's it, we are in system under www-data account. Now we can try to read **user's flag**:
![](13.png)
For privilege escalation we will use an LinPEAS script. Download it from attacker machine and run:
![](14.png)
LinPEAS show us a lot of information like a system info and much more:
![](15.png)
Attention attracts by task scheduler that show us a every-minute running script in /var/www/laravel directory:
![](16.png)
Besides we can spot a credentials to DB, redis etc.:
![](17.png)
Moving to laravel folder shows us many files:
![](18.png)
Artisan file contains an PHP-code that we can edit:
![](19.png)
For further step we need to prepare. Create a php reverse shell file on attacker machine:
![](20.png)
and upload it to cronos machine as artisan file:
![](21.png)
Use netcat to catch incoming connection and wait about one minute:
![](22.png)
Now we're under root account. Let's get **root's flag**:
![](23.png)
