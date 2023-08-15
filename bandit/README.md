# 0
```
❯ ssh bandit0@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password: 

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * peda (https://github.com/longld/peda.git) in /opt/peda/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

 Both python2 and python3 are installed.

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```
# 1
```
❯ ssh bandit1@bandit.labs.overthewire.org -p 2220
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```
# 2
```
❯ ssh bandit2@bandit.labs.overthewire.org -p 2220
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename 
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
```
# 3
```
❯ ssh bandit3@bandit.labs.overthewire.org -p 2220
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cat inhere/.hidden 
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
```
# 4
```
❯ ssh bandit4@bandit.labs.overthewire.org -p 2220
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: Non-ISO extended-ASCII text, with no line terminators
bandit4@bandit:~/inhere$ cat ./-file07
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```
# 5
```
❯ ssh bandit5@bandit.labs.overthewire.org -p 2220
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ cd inhere/
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
bandit5@bandit:~/inhere$ find . -size 1033c               
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2         
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```
# 6
```
❯ ssh bandit6@bandit.labs.overthewire.org -p 2220
bandit6@bandit:~$ ls
bandit6@bandit:~$ find / -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password 
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```
# 7
```
❯ ssh bandit7@bandit.labs.overthewire.org -p 2220
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ cat data.txt | grep millionth
millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
# 8
```
❯ ssh bandit8@bandit.labs.overthewire.org -p 2220
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ cat data.txt | sort | uniq -c | grep '1 '   
      1 EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```
# 9
```
❯ ssh bandit9@bandit.labs.overthewire.org -p 2220
bandit9@bandit:~$ ls
data.txt
bandit9@bandit:~$ strings data.txt | grep ==
4========== the#
========== password
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```
# 10
```
❯ ssh bandit10@bandit.labs.overthewire.org -p 2220
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt 
VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjJSS05kTllGTmI2blZDS3pwaGxYSEJNCg==
bandit10@bandit:~$ base64 -d data.txt 
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```
# 11
```
❯ ssh bandit11@bandit.labs.overthewire.org -p 2220
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt 
Gur cnffjbeq vf WIAOOSFzMjXXBC0KoSKBbJ8puQm5lIEi
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
# 12
```
❯ ssh bandit12@bandit.labs.overthewire.org -p 2220
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ mkdir /tmp/stuff
bandit12@bandit:~$ cp data.txt /tmp/stuff
bandit12@bandit:~$ cd /tmp/stuff
bandit12@bandit:/tmp/stuff$ cat data.txt 
00000000: 1f8b 0808 2773 4564 0203 6461 7461 322e  ....'sEd..data2.
...
bandit12@bandit:/tmp/stuff$ xxd -r data.txt binary.gz
bandit12@bandit:/tmp/stuff$ gzip -d binary.gz
bandit12@bandit:/tmp/stuff$ xxd binary 
00000000: 425a 6839 3141 5926 5359 7b4f 965f 0000  BZh91AY&SY{O._..
...
bandit12@bandit:/tmp/stuff$ bzip2 -d binary
bzip2: Can't guess original name for binary -- using binary.out
bandit12@bandit:/tmp/stuff$ xxd binary.out 
00000000: 1f8b 0808 2773 4564 0203 6461 7461 342e  ....'sEd..data4.
...
bandit12@bandit:/tmp/stuff$ mv binary.out binary.gz
bandit12@bandit:/tmp/stuff$ gzip -d binary.gz
bandit12@bandit:/tmp/stuff$ xxd binary | head      
00000000: 6461 7461 352e 6269 6e00 0000 0000 0000  data5.bin.......
...
bandit12@bandit:/tmp/stuff$ tar -xf binary   
bandit12@bandit:/tmp/stuff$ ls
binary  data5.bin  data.txt
bandit12@bandit:/tmp/stuff$ xxd data5.bin | head
00000000: 6461 7461 362e 6269 6e00 0000 0000 0000  data6.bin.......
...
bandit12@bandit:/tmp/stuff$ tar -xf data5.bin
bandit12@bandit:/tmp/stuff$ xxd data6.bin
00000000: 425a 6839 3141 5926 5359 4911 0aa4 0000  BZh91AY&SYI.....
...
bandit12@bandit:/tmp/stuff$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/stuff$ xxd data6.bin.out | head
00000000: 6461 7461 382e 6269 6e00 0000 0000 0000  data8.bin.......
...
bandit12@bandit:/tmp/stuff$ tar -xf data6.bin.out 
bandit12@bandit:/tmp/stuff$ ls
binary  data5.bin  data6.bin.out  data8.bin  data.txt
bandit12@bandit:/tmp/stuff$ xxd data8.bin 
00000000: 1f8b 0808 2773 4564 0203 6461 7461 392e  ....'sEd..data9.
...
bandit12@bandit:/tmp/stuff$ mv data8.bin data.gz
bandit12@bandit:/tmp/stuff$ gzip -d data.gz
bandit12@bandit:/tmp/stuff$ cat data
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```
# 13
```
❯ ssh bandit13@bandit.labs.overthewire.org -p 2220
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
bandit13@bandit.labs.overthewire.org's password: 
sshkey.private                                                                100% 1679     7.7KB/s   00:00
❯ chmod 0400 sshkey.private
```
# 14
```
❯ ssh bandit14@bandit.labs.overthewire.org -p 2220 -i sshkey.private
bandit14@bandit:~$  cat /etc/bandit_pass/bandit14
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
bandit14@bandit:~$ nc localhost 30000
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```
# 15
```
❯ ssh bandit15@bandit.labs.overthewire.org -p 2220
bandit15@bandit:~$ openssl s_client -connect localhost:30001
...
read R BLOCK
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1
```
# 16
```
❯ ssh bandit16@bandit.labs.overthewire.org -p 2220
bandit16@bandit:~$ nmap -p31000-32000  localhost
Starting Nmap 7.80 ( https://nmap.org ) at 2023-08-15 03:23 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00026s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
bandit16@bandit:~$ openssl s_client -connect localhost:31790
...
read R BLOCK
JQttfApK4SeyHwDlI9SXGR50qclOAil1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
bandit16@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
❯ vim bandit17.private
❯ chmod 0400 bandit17.private
```
# 17
```
❯ ssh bandit17@bandit.labs.overthewire.org -p 2220 -i bandit17.private
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< glZreTEH1V3cGKL6g4conYqZqaEj0mte
---
> hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```
# 18
```
❯ scp -P 2220 bandit18@bandit.labs.overthewire.org:readme .
bandit18@bandit.labs.overthewire.org's password: 
readme                                                                        100%   33     0.2KB/s   00:00    
❯ cat readme
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```
# 19
```
❯ scp -P 2220 bandit19@bandit.labs.overthewire.org:readme .
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```
# 20
```
❯ scp -P 2220 bandit20@bandit.labs.overthewire.org:readme .
```
