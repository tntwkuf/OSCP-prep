Cronos is medium difficulty OSCP-like machine based on Linux.
Let's start our research with a nmap scan to detect open ports, running services and soft version:
![](D:\Git\OSCP-prep\Cronos\1.png)
We can spot an 22, 53 and 80 open ports. The last one is a web-server:
![](D:\Git\OSCP-prep\Cronos\2.png)
Fuzzing directories is don't give us interesting information except URL /web.config:
![](D:\Git\OSCP-prep\Cronos\3.png)
Web.config's content is a rules of webserver that matches end replaces URL with regexp:
![](D:\Git\OSCP-prep\Cronos\4.png)
Further directories fuzzing showed no results unlike subdomains fuzzing:
![](D:\Git\OSCP-prep\Cronos\5.png)
There we can see a subdomain «admin». Let's check it out:
![](D:\Git\OSCP-prep\Cronos\6.png)
Here we can spot an auth panel. Additional checks shows us an SQL-injection that we can use to bypass auth system:
![](D:\Git\OSCP-prep\Cronos\7.png)
and we receive a redirect to /welcome.php:
![](D:\Git\OSCP-prep\Cronos\8.png)
Let's see that page in a browser:
![](D:\Git\OSCP-prep\Cronos\9.png)
It seems like a ping online tool which often vulnerable to command-injections. Let's try to execute any command by adding a semicolon:
![](D:\Git\OSCP-prep\Cronos\10.png)
Now we can do many things such as reading local files with config:
![](D:\Git\OSCP-prep\Cronos\11.png)
Next step is gaining reverse shell with python command and running netcat on our attacker machine:
![](D:\Git\OSCP-prep\Cronos\12.png)
That's it, we are in system under www-data account. Now we can try to read **user's flag**:
![](D:\Git\OSCP-prep\Cronos\13.png)
For privilege escalation we will use an LinPEAS script. Download it from attacker machine and run:
![](D:\Git\OSCP-prep\Cronos\14.png)
LinPEAS show us a lot of information like a system info and much more:
![](D:\Git\OSCP-prep\Cronos\15.png)
Attention attracts by task scheduler that show us a every-minute running script in /var/www/laravel directory:
![](D:\Git\OSCP-prep\Cronos\16.png)
Besides we can spot a credentials to DB, redis etc.:
![](D:\Git\OSCP-prep\Cronos\17.png)
Moving to laravel folder shows us many files:
![](D:\Git\OSCP-prep\Cronos\18.png)
Artisan file contains an PHP-code that we can edit:
![](D:\Git\OSCP-prep\Cronos\19.png)
For further step we need to prepare. Create a php reverse shell file on attacker machine:
![](D:\Git\OSCP-prep\Cronos\20.png)
and upload it to cronos machine as artisan file:
![](D:\Git\OSCP-prep\Cronos\21.png)
Use netcat to catch incoming connection and wait about one minute:
![](D:\Git\OSCP-prep\Cronos\22.png)
Now we're under root account. Let's get **root's flag**:
![](D:\Git\OSCP-prep\Cronos\23.png)