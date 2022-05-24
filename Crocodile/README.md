# Crocodile

![](Misc/crocodile_pwnd.PNG)

This is my eigth machine when learning the basics of penetration testing. Tags in this machine are:

- PHP
- FTP

## <ins>**Questions**

* [questions 1-3](Misc/questions_1.PNG)
* [questions 4-6](Misc/questions_2.PNG)
* [questions 7-9](Misc/questions_3.PNG)

## <ins>**Tasks**

For the first questions you have to use nmap and look the output.

![](Misc/nmap.PNG)

After this you have to use ftp command. From the previous picture we can see that you can get in the ftp as anonymous user so let us try that and see if we can get any files from there.

![](Misc/ftp.PNG)

We got in and can download two files. Let us download those and then see what usernames we got in the file.

![](Misc/usernames_passwords.PNG)

For the last task you have to go to the ip address of the target machine. It opens a website as follows.

![](Misc/website.PNG)

For the last task we used google for gobuster syntax. After finding help for the syntax we got in and found the answer for the last task. It was login.php

![](Misc/gobuster.PNG)

## <ins>**Flag**

To find flag we can try different ip addresses. For example one of the following:

- ip-of-target-machine/assets/
- ip-of-target-machine/js/
- ip-of-target-machine/login.php/

The last ip address gives us the following window to the browser.

![](Misc/login.PNG)

After that we can try different username / password combinations and see if any of those actually works.

Bingo! We got in with the following combination:
~~~
Username: admin
Password: rKXM59ESxesUFHAd
~~~

After that we get to the following window and we see the flag. Now we can submit it and move on to the next machine. Hurray!

![](Misc/flag.PNG)