Cronos is medium difficulty OSCP-like machine based on Linux.
Let's start our research with a nmap scan to detect open ports, running services and soft version:<br>
![](1.png)<br>
We can spot an 22, 53 and 80 open ports. The last one is a web-server:<br>
![](2.png)<br>
Fuzzing directories is don't give us interesting information except URL /web.config:<br>
![](3.png)<br>
Web.config's content is a rules of webserver that matches end replaces URL with regexp:<br>
![](4.png)<br>
Further directories fuzzing showed no results unlike subdomains fuzzing:<br>
![](5.png)<br>
There we can see a subdomain «admin». Let's check it out:<br>
![](6.png)<br>
Here we can spot an auth panel. Additional checks shows us an SQL-injection that we can use to bypass auth system:<br>
![](7.png)<br>
and we receive a redirect to /welcome.php:<br>
![](8.png)<br>
Let's see that page in a browser:<br>
![](9.png)<br>
It seems like a ping online tool which often vulnerable to command-injections. Let's try to execute any command by adding a semicolon:<br>
![](10.png)<br>
Now we can do many things such as reading local files with config:<br>
![](11.png)<br>
Next step is gaining reverse shell with python command and running netcat on our attacker machine:<br>
![](12.png)<br>
That's it, we are in system under www-data account. Now we can try to read **user's flag**:<br>
![](13.png)<br>
For privilege escalation we will use an LinPEAS script. Download it from attacker machine and run:<br>
![](14.png)<br>
LinPEAS show us a lot of information like a system info and much more:<br>
![](15.png)<br>
Attention attracts by task scheduler that show us a every-minute running script in /var/www/laravel directory:<br>
![](16.png)<br>
Besides we can spot a credentials to DB, redis etc.:<br>
![](17.png)<br>
Moving to laravel folder shows us many files:<br>
![](18.png)<br>
Artisan file contains an PHP-code that we can edit:<br>
![](19.png)<br>
For further step we need to prepare. Create a php reverse shell file on attacker machine:<br>
![](20.png)<br>
and upload it to cronos machine as artisan file:<br>
![](21.png)<br>
Use netcat to catch incoming connection and wait about one minute:<br>
![](22.png)<br>
Now we're under root account. Let's get **root's flag**:<br>
![](23.png)
