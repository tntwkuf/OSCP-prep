SIlo is medium difficulty OSCP-like machine based on Windows.
Let's start research with a nmap scan to detect open ports, running services and soft version:<br>
![](1.png)<br>
There we can see a lot of open ports, most interesting is 80, 139, 445 and 1521. Let's check a web server:<br>
![](2.png)<br>
There is a Microsoft IIS server. We can fuzz him to found a directories:<br>
![](3.png)<br>
![](4.png)<br>
But nothing is founded, SMB enumeration is fail too:<br>
![](5.png)<br>
Additionally we can fuzz a subdomains:<br>
![](6.png)<br>
Besides we can check an Orcale TNS Listener version in searchsploit:<br>
![](7.png)<br>
Nothing fits our version. Let's use Oracle Database Attacking Tool to enumerate:<br>
![](8.png)<br>
It founds service name and SID and valid credentials:<br>
![](9.png)<br>
ODAT allows us read and write files, e.g. write reverse shell to server. Let's compile it with msfvenom:<br>
![](10.png)<br>
Later we'll upload reverse shell to server:<br>
![](11.png)<br>
And run it:<br>
![](12.png)<br>
Now we got shell with authority/system privileges:<br>
![](13.png)<br>
It's time to get user flag:<br>
![](14.png)<br>
and root flag:<br>
![](15.png)<br>