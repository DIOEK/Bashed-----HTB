# Bashed-----HTB


└─$ nmap bashed.htb                         
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-11 16:08 -0300
Nmap scan report for bashed.htb (10.129.4.101)
Host is up (0.17s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 7.90 seconds



Only port eighty. Reveals a website:
<img width="1919" height="784" alt="image" src="https://github.com/user-attachments/assets/e3877c13-3b3a-4897-9ed1-e1fc86e0cd8a" />

Let's ffufing the website we find the /dev folder using the following command
ffuf -w ../../SecLists/Discovery/Web-Content/quickhits.txt -u http://bashed.htb/FUZZ
We can find the dev folder:
<img width="794" height="155" alt="image" src="https://github.com/user-attachments/assets/7ced6a12-3020-412b-9d65-eeb6ee3bb30d" />

Inside we find phpbash.php
<img width="1230" height="479" alt="image" src="https://github.com/user-attachments/assets/b883feef-0a72-4fd1-ac6c-ec2306fa1a97" />

Go to /home/arrexel and cat the user
<img width="961" height="762" alt="image" src="https://github.com/user-attachments/assets/9e2b509d-185b-4d49-9e82-1df9e3dc345f" />

nw sudo -l to see that we can execute commands as scriptmanager freely:
<img width="1061" height="117" alt="image" src="https://github.com/user-attachments/assets/4dcce543-0f8d-47de-a495-e78e0caadbfc" />

Now we look for files and folders that belong to scriptmanager
sudo -u scriptmanager find / -user scriptmanager -ls 2>/dev/null







