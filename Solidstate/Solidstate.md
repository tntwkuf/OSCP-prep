Solidstate is medium difficulty OSCP-like machine based on Linux.
Let's start research with a nmap scan to detect open ports, running services and soft version:<br>
![](1.png)<br>
There we can see a lot of open ports, most interesting is 25 and 80. The last one is web server:<br>
![](2.png)<br>
We can fuzz him to find directories<br>
![](3.png)<br>
![](4.png)<br>
and subdomains<br>
![](5.png)<br>
But nothing is founded. We can enumerate SMTP with nmap default scripts:<br>
![](6.png)<br>
We have a one username - root. With knowing version of SMTP (James 2.3.2) we can check vulnerabilities via searchsploit:<br>
![](7.png)<br>
One of them can give us a RCE. Checking his source code show us default credentials for James 2.3.2:<br>
![](8.png)<br>
Running exploit is successfully, but now we need that somebody login in to server to trigger our payload:<br>
![](9.png)<br>
Swift googling reveals that James SMTP usually uses a 4555 port to manage service. Check it with nmap:<br>
![](10.png)<br>
Port is open. Let's login into with known credentials:<br>
![](11.png)<br>
Success. Now we can enumerate users and reset their passwords to read mails:<br>
![](12.png)<br>
With this usernames let's check mails via 110 port:<br>
![](13.png)<br>
Mailadmin tells john that there is a new employee, who needs to send a new credentials:<br>
This employee exists in our usernames list. Check her mailbox:<br>
![](14.png)<br>
Here we get an ssh credentials. Try to login:<br>
![](15.png)<br>
Mindy does not have permissions to take any action, but we got reverse shell already:<br>
![](16.png)<br>
Now we got a user flag. Let's run LinPEAS to check possible privilege escalation vectors:<br>
![](17.png)<br>
We can spot a strange file tmp.py. Check it:<br>
![](18.png)<br>
This script clears /tmp directory. Placing txt files into /tmp show us that script runs regularly. 
We can edit script to get reverse shell when he triggers:<br>
![](19.png)<br>


