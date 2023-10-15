Bastard is medium difficulty OSCP-like machine based in Linux.
Our research started with a nmap scan to detect open ports, running services and soft version:<br>
![](1.png)<br>
There we can see an 80, 135 and 49154 open ports. The last one is a web server:<br>
![](2.png)<br>
Wappalyzer detects a Drupal with version 7.x. Let's check version manually with changelog file:<br>
![](3.png)<br>
Changelog tell us that Drupal version is 7.54. Check its version in Searchsploit:<br>
![](4.png)<br>
There are a lot of vulnerabilities. In our case may be handy a Drupalgeddon2 vulnerability with RCE:<br>
![](5.png)<br>
We got a shell. Let's stable him with reverse shell:<br>
![](6.png)<br>
OK, now we are under authority\iusr account. We can get a user's flag:<br>
![](7.png)<br>
Next step is collect information about system to build a privilege escalation vectors. Let's download on machine winPEAS:<br>
![](8.png)<br>
Is shows us a base system info:<br>
![](9.png)<br>
Additionally we can spot a missing hotfixes:<br>
![](10.png)<br>
After a number of trying a different vulnerabilities we found a MS10-059. Download its exploit to machine:<br>
![](11.png)<br>
And running it to get a reverse shell:<br>
![](12.png)<br>
Success, now we are under authority\system account:<br>
![](13.png)<br>
Let's get a root flag:<br>
![](14.png)