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






