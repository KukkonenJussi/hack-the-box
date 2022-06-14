# Archtype

This is my ninth machine when learning the basics of penetration testing. Tags included in this machine are:

- SMB
- SQL

## <ins>**Questions**
* [Questions 1-4](Misc/questions_1.PNG)

## <ins>**Tasks**

We start by scanning the ports. We can see what ports and services are open.

![](Misc/nmap.PNG)
![](Misc/nmap_2.PNG)

For the next task we have to use smbclient so let us read some manuals and see which switch do we could use to get some answers for the next task(s).

We could try to get in with the following command. Switch -N do not ask for a password" and -L gets a list of shares that are available on a host. 

~~~
smbclient -N -L
~~~

![](Misc/smbclient.PNG)

Then we have to get in with sambaclient. Remember the funky syntax for getting in. Here is an example how we can get in.

![](Misc/smbclient_in.PNG)

Now we can use basic Linux commands to navigate through the folders and find some information.

Seems like the prodprod.dtsConfig is the only file we can find in the backups folder. So let us get that with get <filename> command and exit the database.

Now we have to open the file we got. Let us use google and hopefully get some answers.

Seems that we can open it normally without any problems. So let us use cat command and see the content.

![](Misc/file.PNG)