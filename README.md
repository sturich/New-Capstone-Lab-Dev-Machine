<h1>Dev Machine</h1>

<h2>**IP Address provided: 10.0.2.8**</h2>

This repository provides a comprehensive guide to the Academy Machine which is a Capture the Flag (CTF) challenge designed to test your skills in network scanning, web application testing, password cracking, privilege escalation, and system exploitation. This machine provides an opportunity to utilise a variety of tools and techniques commonly used in real-world penetration testing. You will encounter a combination of web-based vulnerabilities, system misconfigurations, and network-related challenges.

### Features:
- NMAP (Network Mapper)
NMAP is used for network scanning, identifying live hosts, and enumerating open ports/services on the target machine. Expect to perform full-service enumeration and version detection to discover potential vulnerabilities.
- Hashcat
Hashcat is employed to crack passwords or hash values found during the challenge, such as those retrieved from configuration files or databases.
- Dirb/Dirbuster/FFUF
These tools will be used for web directory and file brute-forcing to discover hidden resources or services within the web application. Expect to uncover directories with potentially sensitive data or misconfigurations.
- PHP Reverse Shell
The PHP reverse shell allows for remote code execution. You will use this shell to establish a reverse connection back to your local machine after exploiting a vulnerable web application or misconfiguration.
- Linpeas (Linux Privilege Escalation Enumeration)
Linpeas will help you scan for potential privilege escalation vectors once you've gained initial access to the system. It will identify misconfigurations, setuid binaries, and other weaknesses that can be leveraged for escalating privileges.
- Pspy64
Pspy64 is used to monitor the processes running on the target system in real time. It can help identify scheduled tasks, cron jobs, or other processes that might be running with elevated privileges, making it a valuable tool for post-exploitation.
- Bash Reverse Shell One-Liner
The bash reverse shell one-liner will be useful when you need to quickly establish a reverse shell connection, bypassing firewalls and security measures.
- Netcap (Network Capture)
Netcap will capture network traffic, allowing you to analyse packets, sniff traffic, and uncover hidden information such as unencrypted credentials, flags, or other clues that might assist you in further exploiting the system.


### Prerequisites:
Before starting the Academy Machine challenge, it is recommended to have basic knowledge and hands-on experience with the following tools and concepts:

- Networking Fundamentals: Knowledge of TCP/IP, ports, and protocols.
- NMAP: For network discovery and service enumeration.
- Hashcat: For password cracking and hash analysis.
- Dirb/Dirbuster/FFUF: Web directory and file brute-forcing tools.
- PHP Reverse Shell: Using PHP scripts to initiate reverse shells for exploitation.
- Linpeas: Linux privilege escalation enumeration script.
- Pspy64: To monitor processes running on the machine.
- Bash Reverse Shell One-Liner: Executing reverse shell using bash scripting.
- Netcap: For monitoring and capturing network traffic, analysing packets for clues.

<h2>Environments Used </h2>

- <b>Linux</b>

<h2>Walk-through:</h2>

IP Address = 10.0.2.8

Started out by using Nmap to dearch for open ports on the target 10.0.2.8

nmap -A -p- -T4 10.0.2.8

└─# nmap -A -p- -T4 10.0.2.8  
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-04 12:51 EDT  
Nmap scan report for 10.0.2.8  
Host is up (0.00051s latency).  
Not shown: 65526 closed tcp ports (reset)  
PORT STATE SERVICE VERSION  
22/tcp open ssh OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)  
| ssh-hostkey:  
| 2048 bd:96:ec:08:2f:b1:ea:06:ca:fc:46:8a:7e:8a:e3:55 (RSA)  
| 256 56:32:3b:9f:48:2d:e0:7e:1b:df:20:f8:03:60:56:5e (ECDSA)  
|_ 256 95:dd:20:ee:6f:01:b6:e1:43:2e:3c:f4:38:03:5b:36 (ED25519)  
80/tcp open http Apache httpd 2.4.38 ((Debian))  
|_http-server-header: Apache/2.4.38 (Debian)  
|*http-title: Bolt - Installation error  
111/tcp open rpcbind 2-4 (RPC #100000)  
| rpcinfo:  
| program version port/proto service  
| 100000 2,3,4 111/tcp rpcbind  
| 100000 2,3,4 111/udp rpcbind  
| 100000 3,4 111/tcp6 rpcbind  
| 100000 3,4 111/udp6 rpcbind  
| 100003 3 2049/udp nfs  
| 100003 3 2049/udp6 nfs  
| 100003 3,4 2049/tcp nfs  
| 100003 3,4 2049/tcp6 nfs  
| 100005 1,2,3 42304/udp mountd  
| 100005 1,2,3 51225/tcp mountd  
| 100005 1,2,3 54449/udp6 mountd  
| 100005 1,2,3 60027/tcp6 mountd  
| 100021 1,3,4 36437/tcp nlockmgr  
| 100021 1,3,4 41493/tcp6 nlockmgr  
| 100021 1,3,4 48225/udp6 nlockmgr  
| 100021 1,3,4 52438/udp nlockmgr  
| 100227 3 2049/tcp nfs_acl  
| 100227 3 2049/tcp6 nfs_acl  
| 100227 3 2049/udp nfs_acl  
|* 100227 3 2049/udp6 nfs_acl  
2049/tcp open nfs 3-4 (RPC #100003)  
8080/tcp open http Apache httpd 2.4.38 ((Debian))  
|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()  
| http-open-proxy: Potentially OPEN proxy.  
|_Methods supported:CONNECTION  
|_http-server-header: Apache/2.4.38 (Debian)  
36437/tcp open nlockmgr 1-4 (RPC #100021)  
38473/tcp open mountd 1-3 (RPC #100005)  
51225/tcp open mountd 1-3 (RPC #100005)  
57889/tcp open mountd 1-3 (RPC #100005)  
MAC Address: 08:00:27:9F:6B:61 (Oracle VirtualBox virtual NIC)  
Device type: general purpose  
Running: Linux 4.X|5.X  
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5  
OS details: Linux 4.15 - 5.8  
Network Distance: 1 hop  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

From this we will be interested in the following ports: 80/ 8080 / 2049 we could look at 111 RPC but not at this level yet.

So for port 8080 go out to the webpage 10.0.2.8

![image](https://github.com/user-attachments/assets/b134ac3f-6530-4bce-921d-2a835d16cf47)

We can click on the link to go to the official site, we can see there is a good chance it is running on Bolt cms

![image](https://github.com/user-attachments/assets/a2f48ba3-fc73-4d4d-8786-f5c3514d485c)

We can also see that the file is missing in the location, this location is a Linux location so it looks like we may be up against a Linux machine

![image](https://github.com/user-attachments/assets/dc78831a-3487-4bbb-8560-ecf14dffc0b0)

If we add port 8080 to the web address we get this page.

![image](https://github.com/user-attachments/assets/640d16dc-9604-4910-a381-ce9fc90b1e3f)
![image](https://github.com/user-attachments/assets/d6e6487c-ca6f-4c18-8681-50c7e9fa6d75)

This is a php info page, sometimes this will disclose some information.

![image](https://github.com/user-attachments/assets/ae009d6c-27f4-4bf8-be1f-2e683ea97d0b)

We can see the path, the webmaster.

Now if were attacking this page we could find files, or there could be a setting misconfiguration.

So we want to enumerate this a bit further to see.

So we are going to carry out a quick scan using fuff

![image](https://github.com/user-attachments/assets/680ee934-1486-4fac-bd7e-fef5951233e5)

Open up a new tab and run the same command with port 8080

![image](https://github.com/user-attachments/assets/e379c9a1-b328-4622-8c7c-52ddfa9bf554)

While these are running we are going to look at port 2049 open nfs  
Its important to multi task and have numerous scans running at once to reduce the time required, if you have a week to look at a thousand IP addresses then you need to maximise your time.

![image](https://github.com/user-attachments/assets/74dd124c-ce25-4b31-b6e2-339d90b509f3)

So we are going to mount and see what we can see in this share.  
In order to do that we need to make a directory to mount this to.

![image](https://github.com/user-attachments/assets/f05e9ff6-0a5e-453e-9ff3-5740735eea92)

![image](https://github.com/user-attachments/assets/bac53e12-8b33-4fdc-9e3a-ed841b762fcd)

cd into mnt/dev, there is a file called save.zip

![image](https://github.com/user-attachments/assets/3a031169-fbf6-43b4-9ec9-f175af7b497c)

unzip the file

![image](https://github.com/user-attachments/assets/7e4126ec-baf2-451b-9bf5-c5340a138eed)

we dont know the password so press enter, we can see there are 2 files in here.

![image](https://github.com/user-attachments/assets/9bc0b8dd-5ed2-4147-8316-4c002bbeb1cb)

we can try to crack the password using fcrackzip on Linux, install onto Linux.

![image](https://github.com/user-attachments/assets/02d925bd-46fe-4879-ab4a-25b935543892)

Once installed we will run this against the file.

![image](https://github.com/user-attachments/assets/d6bc5173-20e6-4012-a170-085d14aed60c)

-v = verbose, we want to see detail.  
-u = unzip  
-D = use a wordlist (we will use rock you.txt)  
-p = file

![image](https://github.com/user-attachments/assets/7a1a3173-1862-44ff-94c2-484b8bfac269)

so we have password is equal to java101

now if we go to unzip save.zip and enter the password.

![image](https://github.com/user-attachments/assets/a427db2b-f6ba-4251-8922-182f533b2b7d)

ls

![image](https://github.com/user-attachments/assets/ce794527-1f06-4593-8daf-3c80f9ffadd2)

lets cat out the todo.txt

![image](https://github.com/user-attachments/assets/73bfb25b-1ef7-45c7-a394-8846e5231c0f)

we can see a signature of jp but we dont know who that is, we can see a file called id_rsa, that id_rsa file csn be used to connect via ssh.

So we can try ssh -i id_rsa jp@IP address, it then asks for the password which we don't know so we try enter.

![image](https://github.com/user-attachments/assets/b1770ec8-4013-4728-a6d0-992bf80df3c8)

we don't know the password, we don't know if this person is a user, so we hold off on this and go back to our scans.

on the port 80 scan.

![image](https://github.com/user-attachments/assets/f15368dd-5700-49b6-b8b8-67a86543a2a3)

we have

![image](https://github.com/user-attachments/assets/79dd02ff-d9e4-4594-ae4d-01df371030fd)

we also have a dev directory so if we add dev to the IP address we are brought to this page.

![image](https://github.com/user-attachments/assets/e54ab5c6-cf91-482c-a9bd-c0f984859b66)

![image](https://github.com/user-attachments/assets/ef7613bf-13b3-498e-8fcf-ba65707d8485)

not much information there, try the public, src and app.

On app we find a directory.

![image](https://github.com/user-attachments/assets/304edd8a-9748-415d-b3e9-b5e8d9a22096)

Potentially there could be a SQL data base and that could contain all the passwords so its good to check everything.

We go into the config files and download the config.yml file.

![image](https://github.com/user-attachments/assets/1524c6f0-43ff-4029-8cea-04b19fa537c4)

Open this file up.

![image](https://github.com/user-attachments/assets/4f01b2c1-7337-40ac-aa57-298b104fe51b)

open with mousepad.

![image](https://github.com/user-attachments/assets/7eaa1003-c403-4aa2-93ac-ff56f225bd75)

![image](https://github.com/user-attachments/assets/c896dfdb-2c2e-4c0a-86cf-cf37894822fc)

we can see the username and password, copy this.

![image](https://github.com/user-attachments/assets/970b500e-08ef-409f-b0b3-7baa881c398a)

so we look at the boltwire and see if there is any exploits.

![image](https://github.com/user-attachments/assets/9fddd1a7-600b-433b-b6cb-1ce5c5383f9e)

We can also do on Linux

![image](https://github.com/user-attachments/assets/a190d927-f29b-451f-bb85-97d7ab21fc0a)

one thing that shows up is multiple cross site scripting which is not going to be a benefit to use unless there is someone mimicking a user and going in and actually navigating the website, there's no use for attacking cross site scripting. that's not to say this is not a great attack, but this is to attack actual real users, we are not attacking any users, we are attacking the box.

So maybe we will focus on the local file inclusion.

![image](https://github.com/user-attachments/assets/e6622de0-476a-4f53-a3b0-ce98125cc22b)

![image](https://github.com/user-attachments/assets/bbb5c335-cdec-43bc-9375-c10811b439c3)

a local file inclusion potentially allows us to expose files that are running on a server and this can lead to information disclosure, remote code execution.  
So what's happening here is it is excepting this path here as input, its a coding error on the development side.  
So if we think about this input as a Linux machine we are basically saying ../ lots of times to get back as far as possible and then etc./password. the ../ can be put in as many times as you want, you could put 10 in and it may work.

![image](https://github.com/user-attachments/assets/49022086-9cb6-4a97-8414-dbaae7135873)

NOTE: make sure you have created an account on the boltwire site or this wont work. we can see the http address that we used for the site with the additional information so type this in.

This works.

![image](https://github.com/user-attachments/assets/e2bc2a9a-dbac-441b-8d87-549dd06b85c6)

We can try running as shadow instead of password, on this occasion we can read the shadow so we are on a user that can read the etc./password only.  
So if we scroll down to look for users we only find one which is JP.

![image](https://github.com/user-attachments/assets/4cd66087-6304-4145-966e-093097f38633)

so this would explain we were not able to login as JP as the user is Jean Paul.

Lets go back into this file and try as Jean Paul instead of jp

![image](https://github.com/user-attachments/assets/f54d6379-4bc6-4997-b7ca-ef46336e3fb0)

its asking for a password key phrase

![image](https://github.com/user-attachments/assets/958942e1-e70e-4ffd-94a4-2a50cd53d5b4)

so we have 2 options here, Brute force the id_rsa using john the ripper, the issue is if the password doesnt show in your list or the password is too strong then its not going to work.

or

We have some information that we found out earlier, we know in the text that Jean Paul said in his text file keep coding in java, its awesome and we seen a password with username bolt I_love_java.  
Chances are this password wouldnt be in a list but we can check

![image](https://github.com/user-attachments/assets/6faaa16d-49fb-4094-afd6-b520b0990b62)

and it doesnt.  
So we will try the password we found.

![image](https://github.com/user-attachments/assets/9b95c093-35f2-4100-b532-63a1ea1c6a03)

And it worked, we are IN!!!!  
We are now the low level user.

We do ls and pwd, we are in his home folder but theres nothing here.

![image](https://github.com/user-attachments/assets/b9f501d2-049f-435e-aee0-b6f200aa7974)

Lets do a history check to see what commands have been run recently.

![image](https://github.com/user-attachments/assets/cec14d1f-355e-4ab8-8ef2-57e88abed9f4)

So we run a sudo -l as this tells us what we can run as sudo without a password.

![image](https://github.com/user-attachments/assets/4e3fa8a3-4378-4a24-b4f5-7e999fd5b717)

So we can see that we can run the zip as sudo without password, so we run that.

![image](https://github.com/user-attachments/assets/993c42df-9277-4df4-8a9a-4c7c1a2ef47a)

![image](https://github.com/user-attachments/assets/0f546729-ab1c-46eb-a99a-9359e89741fe)

SO!! how can we take sudo zip and abuse that to allow us to escalate into root?? So there is a site for that called gtfobin.

![image](https://github.com/user-attachments/assets/e3f7c669-feb1-4077-afc7-6ca314604934)

Great website.

![image](https://github.com/user-attachments/assets/2b0ebcd7-e42d-4d65-a5ac-617979a8bef3)
![image](https://github.com/user-attachments/assets/01ff218f-5438-42f5-9eba-2d5df5512a56)

We have a sudo escalation.

![image](https://github.com/user-attachments/assets/f4bf970b-be65-4286-b980-105451983390)

we then search for zip.

![image](https://github.com/user-attachments/assets/c0a2e449-e7be-4904-9a1a-6cc8a504d687)

![image](https://github.com/user-attachments/assets/7974e397-c523-4099-b83b-b7c9ff03e16f)

All we need to do is copy the first line and paste it in.

![image](https://github.com/user-attachments/assets/492535b2-b088-4f4d-b6fd-beb7364c4cd8)

When we type in id we are now root.

![image](https://github.com/user-attachments/assets/47085ea1-cfe3-4388-abc5-15de83ca4336)

We can now cd into root and ls.

![image](https://github.com/user-attachments/assets/b097fe8b-7c7c-4445-b772-c32002743692)

Cat out the flag.txt.

WE HAVE ROOTED THIS BOX.





















