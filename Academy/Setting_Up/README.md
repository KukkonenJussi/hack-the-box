<h4><u>Note Taking</u></h4>

Let us assume that we need to perform an internal penetration test.

1. Before we go to the company, we regenerate a new Secret Key and Master Password and make sure that the 2FA function is enabled.

2. Once we are at the company's internal host, we can install the 1Password add-on for Firefox and log in with the newly generated keys (which we can view from our smartphone or laptop). Even if this host is compromised and a keylogger runs on it, our vault cannot be used by third parties without our 2FA code.

3. Once we are logged in, we can log in with the Firefox account credentials stored in 1Password, and all our add-ons and bookmarks will be available in a few seconds.

<h4><u>Note Taking</u></h4>

As a part of penetration testing, taking notes is very important because we get a lot of different information. The five main types of information are: **_Discovered information, ideas for further tests and processing, scan results, logging and screenshots_**.

- **_Discovered information_**, such as IP addresses, usernames, passwords, source code, etc., is information that we can use against our target company. We often obtain such information through OSINT, active scans, and manual analysis of the given information resources and services.

- **_Ideas for further tests and processing_** - We should note down everything that we have tried and should be investigated. This is because one will have to adapt the approach. One honorable markdown editor for this is [Obsidian](https://obsidian.md/).

- **_Scan results_** - A large amount of information as a result of scans and tests can feel overwhelming. Through practice one can recognize essential piece of information that may be helpful. For this purpose one can use e.g. [Ghostwriter](https://github.com/GhostManager/Ghostwriter).

- **_Logging_** is essential for both documentation and our protection. If third parties attack the company during our penetration test and damage occurs, we can prove that the damage did not result from our activities. For this, we can use the tools `script` and `date`. `Date` can be used to display the exact date and time of each command in our command line. With the help of `script`, every command and the subsequent result is saved in a background file. To display the date and time, we can replace the `PS1` variable in our `.bashrc` file with the following content.

  `PS1="\[\033[1;32m\]\342\224\200\$([[ \$(/opt/vpnbash.sh) == *\"10.\"* ]] && echo \"[\[\033[1;34m\]\$(/opt/vpnserver.sh)\[\033[1;32m\]]\342\224\200[\[\033[1;37m\]\$(/opt/vpnbash.sh)\[\033[1;32m\]]\342\224\200\")[\[\033[1;37m\]\u\[\033[01;32m\]@\[\033[01;34m\]\h\[\033[1;32m\]]\342\224\200[\[\033[1;37m\]\w\[\033[1;32m\]]\n\[\033[1;32m\]\342\224\224\342\224\200\342\224\200\342\225\274 [\[\e[01;33m\]$(date +%D-%r)\[\e[01;32m\]]\\$ \[\e[0m\]"`

  This works in Linux, but in Windows machines you have to use `Start-Transcript`

  `Start-Transcript -Path "C:\Pentesting\03-21-2021-0200pm-exploitation.log"`

TLDR; Logging to a separate file in Linux. `./custom-tool.py 10.129.28.119 > logs.custom-tool`

And in Windows `.\custom-tool.ps1 10.129.28.119 > logs.custom-tool`

- **_Screenshots_** serve as a momentary record and represent proof of results obtained, necessary for the Proof-Of-Concept and our documentation. One of the best tools for this is [Flameshot](https://github.com/flameshot-org/flameshot). It has all the essential functions that we need to quickly edit our screenshots without using an additional editing program. We can install it using our APT package manager or via download from Github.

<h4><u>Virtualization and containers</u></h4>

**Virtual machines** offer numerous advantages over running an operating system or application directly on a physical system. The most important benefits are:

1. Applications and services of a VM do not interfere with each other
2. Complete independence of the guest system from the host system's operating system and the underlying physical hardware
3. VMs can be moved or cloned to other systems by simple copying
4. Hardware resources can be dynamically allocated via the hypervisor
5. Better and more efficient utilization of existing hardware resources
6. Shorter provisioning times for systems and applications
7. Simplified management of virtual systems
8. Higher availability of VMs due to independence from physical resources

A **container** cannot be defined as a virtual machine but as an isolated group of processes running on a single host that corresponds to a complete application, including its configuration and dependencies. This application is packaged in a precisely defined and reusable format. Unlike a usual VM on VMware Workstation, however, a container does not contain its operating system or kernel. It is, therefore, not a virtualized operating system. For this reason, containers are significantly slimmer than conventional virtual machines. Precisely because they are not real virtual machines, they are also referred to as application virtualization in this context. One software that can isolate applications in containers is [Docker](https://www.docker.com/get-started/).

[Vagrant](https://www.vagrantup.com/) is a tool that can create, configure and manage virtual machines or virtual machine environments. The VMs are not created and configured manually but are described in code in a **_Vagrantfile_**. To better structure the program code, the Vagrant file can include additional code files. The code can then be processed using the Vagrant CLI. In this way, we can create, provision, and start our own VMs. Moreover, if the VMs are no longer needed, they can be destroyed just as quickly and easily. Out of the box, Vagrant offers providers for VMware and Docker.
