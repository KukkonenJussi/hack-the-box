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

#### **Gobuster**

![](Misc/gobuster.PNG)

#### **Website**

![](Misc/website.PNG)

![](Misc/subdomain.PNG)

![](Misc/hosts.PNG)

Keep in mind that the ip address of thetoppers.htb changes because I didn't complete the whole machine in one sitting.

![](Misc/images.PNG)

## <ins>**Subdomain enumeration**

![](Misc/vhost.PNG)

## <ins>**AWS Services**


#### **S3 buckets**

![](Misc/aws_configure.PNG)

![](Misc/aws_endpoint.PNG)


#### **Setting up the listeners**

![](Misc/php.PNG)

![](Misc/aws_endpoint_2.PNG)

![](Misc/listener.PNG)

![](Misc/server.PNG)

![](Misc/shell.PNG)

![](Misc/id.PNG)

![](Misc/thetoppers.PNG)


## <ins>**Web shells & Reverse shells**

![](Misc/tun0.PNG)

![](Misc/flag.PNG)

And there we have the flag. Now can submit it and move on to the next machine. Hurray!