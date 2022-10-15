:bulb: 

:::success
# Web Servers and Directory Service

**Name: Emeka Michael Nzeopara**
:::



## Task 1 - Install & Configure Virtual Hosts

:::info
1. Download, verify, build and install the webserver daemon from the source. 
Note: Some features of your web server may be built-in or modularized. Enable at least SSL/TLS during your installation.

:::

To get started with this task, I will download the source file of Nginx and verify that the downloaded file is not compromised. 
Following these procedure by running these commands 

```
* wget https://nginx.org/download/nginx-1.20.2.tar.gz
```

<center>
    
![](https://i.imgur.com/10OlfFb.png =600x)
Figure 1: Nginx Download
</center>

To download the signature that would be used to run this verification, I will run this command
```
* wget https://nginx.org/download/nginx-1.20.2.tar.gz.asc
```

<center>

![](https://i.imgur.com/X2MGp2Z.png =600x)
Figure 2: Signature file for Nginx
</center>

Next I will need to download the public keys that would be needed to verify the signature. From little google search I was able to find the link of the key to be located in [Nginx Source File Key](https://nginx.org/keys/mdounin.key)

Using the following command

```
* wget https://nginx.org/keys/mdounin.key
```

<center>

![](https://i.imgur.com/Ro0HvAc.png =600x)
Figure 3: Key for Verification
</center>

After the download of this key i will import the public key from it that would be used for verification.
To do this I will use this command

```
* gpg --import mdounin.key 
```

<center>

![](https://i.imgur.com/Tzd02GR.png =600x)
Figure 4: Importing of Public Keys
</center>

Since I have performed the necessary steps to verify the file, I will run the command now to verify the source file that i have downloaded

```
* gpg --verify nginx-1.20.2.tar.gz.asc nginx-1.20.2.tar.gz
```

<center>

![](https://i.imgur.com/CidW2LJ.png =600x)
Figure 5: Verification result of Nginx Source File
</center>

To install the nginx webserver, there are usually some dependencies that are appropriate to help with this installation. I will run a command in order to implement these dependencies

```
* sudo apt install -y build-essential zlib1g zlib1g-dev libssl-dev libpcre3 libpcre3-dev
```
<center>

![](https://i.imgur.com/2KJRLYW.png =600x)
Figure 6: Installing Nginx Dependencies
</center>

For me to install the package, i would need to run a bash script 'configure' that comes with the source file when after the extraction. By default most of the file required by Nginx will be kept in the /user/local directory. 
To enable SSL/TLS during the installation, there are some flag i would include, the below command will help in understanding it. This flag is **--with-http_ssl_module**
To build, I will run the following commands

```
* ./configure --with-http_ssl_module --with-pcre
* make
* sudo make install
```

<center>

![](https://i.imgur.com/YbiG2l0.png =600x)
Figure 7: Final Stage of the Build/Install of Nginx
</center>

After the installation has been completed and the binaries created, I had to move the executable to a /usr/local/sbin folder so that the command could be accessible from anywhere in the system bu adding the 'sudo' command to 'nginx'.

We can also notice that the prefix directory for this installation is placed in the **/usr/local/nginx** directory. These information are important to help us understand how we will segment the configuration of the whole nginx.

Let's verify that our Nginx has installed by running the following command.

```
* sudo nginx -v
```
<center>

![](https://i.imgur.com/Ndm0bAb.png =600x)
Figure 8: Running the Built Command from anywhere within the system
</center>

:::info
2. Define the root directory and then two virtual hosts (and configure DNS records or wildcard accordingly):
* aaa.stX.sne22.ru
* bbb.stX.sne22.ru
:::

From the installation process that has been completed, the directory that deals with serving webpages in ubuntu machine is found in the **/var/www/**
I will create a directory that would keep the server files for each of the virtual hosts and name it **sites-available** 

So by placing this directory in the configuration file of Nginx, as shown in the snapshot below

<center>

![](https://i.imgur.com/aY5l5km.png =600x)
Figure 9: Setting Up the Root Directory
</center>

To configure the DNS record, I will update the A record for the DNS zones to have the webserver directory that would be needed.

<center>

![](https://i.imgur.com/wItnVT1.png =600x)
Figure 10: DNS Record for Virtual Hosts
</center>

To configure the two virtual hosts that will be using the Nginx webserver, I will create the directory that the webserver will interact with anytime for these hosts.

```
* sudo mkdir -p /var/www/aaa.st17.sne22.ru/html
* sudo mkdir -p /var/www/bbb.st17.sne22.ru/html
```
After creating this directory, I will assign ownership of the directory with the $USER environment variable

```
* sudo chown -R $USER:$USER /var/www/aaa.st17.sne22.ru/html
* sudo chown -R $USER:$USER /var/www/bbb.st17.sne22.ru/html
```

I will also run a command to modify the permission of the file, although this may not needed if the mode of the file is appropriate

```
* sudo chmod -R 755 /var/www/aaa.st17.sne22.ru/
* sudo chmod -R 755 /var/www/bbb.st17.sne22.ru/
```

<center>

![](https://i.imgur.com/UXUvAQC.png =600x)
Figure 11: Permission modified for the hosting files
</center>
:::info
3. Create a simple, unique HTML page for each virtual host to make sure that the server can correctly serve it.
:::

To create a simple unique HTML page for each Virtual host, I will create an **index.html** file in the directory of the domain.
This is achieved by running the following command

```
* sudo nano /var/www/aaa.st17.sne22.ru/html/index.html
```
<center>

![](https://i.imgur.com/DtaZgEm.png =600x)
Figure 12: Homepage for aaa.st17.sne22.ru domain
</center>

Creating a unique html page for the bbb.st17.sne22.ru domain, we will go by running the following command and then create the following file

```
* sudo nano /var/www/bbb.st17.sne22.ru/html/index.html
```
<center>

![](https://i.imgur.com/Do4YqWH.png =600x)
Figure 13: Homepage for bbb.st17.sne22.ru domain
</center>

I have created the unique html pages. The next step is to make sure that the webserver is able to host these domains. We can achieve these by creating a seperate configuration file for both domains. I will make sure I create these configuration file in the directory specified by the snapshot of **Figure 9**. This would enable the configuration file to be able to find it and serve it 

Using the following command

```
* sudo nano /usr/local/nginx/conf/sites-available/aaa.st17.sne22.ru.conf
```

<center>

![](https://i.imgur.com/klfLXpN.png =600x)
Figure 14: aaa domain server block file
</center>

I will repeat the process and create the same server block file for the bbb domain.

Using the similar command

```
* sudo nano /usr/local/nginx/conf/sites-available/bbb.st17.sne22.ru.conf
```

<center>

![](https://i.imgur.com/39qT7rg.png =600x)
Figure 15: bbb domain server block
</center>

To verify that the aaa domain is working, I will run try browsing from the browser

<center>

![](https://i.imgur.com/UlOJmmx.png =600x)
Figure 16: Web Browser Result for AAA Domain
</center>

To verify that the bbb domain is working too, and running from the browser

<center>

![](https://i.imgur.com/IT9SxRn.png =600x)
Figure 17: Web Browser Result for BBB Domain
</center>

:::info
4. Check the configuration syntax, start the daemon and enable it at boot time.
:::

To achieve this task, I would have to create a bash script that would run at bootime of the operating system. This can be achieve by creating a bash script taking note of the directory that my nginx is located for this.

I will create this bash script in my /etc/init.d directory and run a command to update that directory record to run when the system boots

<center>

![](https://i.imgur.com/fTmf75N.png =600x)
Figure 18: Bash Script for Starting Nginx
</center>

After saving the file you will have to make the script an executable.

```
* sudo chmod +x nginx-starter.sh

```
<center>

![](https://i.imgur.com/UQzZ1Qi.png =600x)
Figure 19: Making the Start-Up Script Executable
</center>

For the final i will run the command that would enable this execution to be possible 

```
* sudo update-rc.d nginx-starter.sh defaults
```

This final command will make nginx to run at boot time of the operating system.

:::info
5. Use curl to display the contents of a full HTTP/1.1 session served by your server.
:::

For the first instance for the first domain

<center>

![](https://i.imgur.com/6OIcTkp.png =600x)
Figure 20: Curl Result of AAA domain
</center>

For the second instance, the curl of the BBB domain

<center>

![](https://i.imgur.com/kp9Kn8O.png =600x)
Figure 21: Curl Result of BBB domain
</center>

:::info
6. Explain the meaning of each request and reply header.
:::

From my research it was found out that http header is divided into three sections

* **General Header:** This is the header for the whole message during the communcication
* **Request Header:** This header modifies the request by giving it context or by applying some conditions to the header
* **Representation Headers:** This describes the encoding and the data of the file that was sent 

**HTTP REQUEST HEADER**

* **GET:** This specify the method that a file is gotten from the webserver
* **Host:** This header gives the domain name that is being queries when the request is made
* **User-Agent:** This describes the application or protocol used to mmake the action. They allow the servers to identify the application, operating system etc.

**HTTP RESPONSE HEADER**

* **HTTP/1.1:** This is the protocol version usually HTTP/1.1
* **200:** Means the connection was successful to the server
* **Server:** This gives the information about which websever that is being used
* **Date:** Shows the dates that the reponse was gotten
* **Content-Type** Shows the media format of the entity-body sent to the recipient
* **Content-Length:** Describe the size of this entity-body
* **Last-Modified:** Usually different from other times and it is the date and time that tje origin server believes the variant was last modified
* **Connection:** This gives the sender options that would probably specify what is desired for the connection
* **ETag:** This is the entity tag in current value
* **Accept-Ranges:** Allows the server to indicate its acceptance of range request for resource

---

## Task 2 - SSL/TLS

:::info
1. Enable SSL/TLS and tune the various settings to make it as secure as possible
:::

SSL/TLS is a security application that is depends on the protocol and the cipher that  enables it, it also has a certificate which correspond to the private key.

I will be using the different version of TLS and SSL to setup my security for Nginx. 
Checking the configuration file, for nginx and adding the following lines of configurations to tune it. In order to make it as secure as possible.

<center>

![](https://i.imgur.com/MbM0Utz.png =600x)
Figure 22: Setting Up SSL and Ciphers to Use
</center>

For the case of this project, the following ciphers are used in these ways. The first cipher is used for key exchange, the second is used for 
domain certificate it enables that every public key has a certain type of algorithm, the third is for the transport cipher and the last term is for messages and caches

<center>

![](https://i.imgur.com/d1rhJce.png =600x)
Figure 23: Cipher Suite Breakdown
</center>

It is also essential to include "ssl_prefer_server_ciphers" so that the setup/nginx can determine which of the cipher is good at a given time.

These are some of the ways to increase the security of the server.

<center>

![](https://i.imgur.com/V06YxHw.png =600x)
Figure 24: Additional setup for SSL
</center>

By completing this, I have been able to complete the requirement of this task number.

:::info
3. Describe how you created your own certificate(s) e.g. with Let’s encrypt (certbot) or self-signed and re-validate every virtual-host . Explain your security tuning process.
:::

To enable SSL/TLS Connection, I will be needing a certificate and keys for my domains. This process will be obtained by using openssl or certbot as suggested. Although certbot will give a trusted certificate that is suitable for application hosted on the internet, but in this case it might not work, so we will use a self-signed certificate by openssl.

I already have openssl installed in my machine, 

<center>

![](https://i.imgur.com/hgVz6hn.png =600x)
Figure 25: Openssl Installed
</center>

```
* sudo openssl genrsa -des3 -out aaa.st17.sne22.ru.key 2048 
* sudo openssl genrsa -des3 -out bbb.st17.sne22.ru.key 2048
```
<center>

![](https://i.imgur.com/NzpcBji.png =600x)
![](https://i.imgur.com/diKFAuC.png =600x)

Figure 26: Key Generated for Each Domain
</center>

The next process is using the key to sign the certificates that would be used

```
* sudo openssl req -new -key aaa.st17.sne22.ru.key -out aaa.st17.sne22.ru.csr
* sudo openssl req -new -key bbb.st17.sne22.ru.key -out bbb.st17.sne22.ru.csr

```


<center>

![](https://i.imgur.com/iSPYiWI.png =600x)
Figure 27: Certificate Generated
</center>

The next step is to generating a self-signed certificate, which will be done by running the following command

```
* sudo openssl x509 -req -days 365 -in aaa.st17.sne22.ru.csr -signkey aaa.st17.sne22.ru.key -out aaa.st17.sne22.ru.crt 
* sudo openssl x509 -req -days 365 -in bbb.st17.sne22.ru.csr -signkey bbb.st17.sne22.ru.key -out bbb.st17.sne22.ru.crt 

```

<center>

![](https://i.imgur.com/r8HUQTJ.png =600x)
Figure 28: Self-Signed Certificate generated
</center>

The next thing I want to do is to disable to password feature so that it will not request it at every instance of login

```
* sudo openssl rsa -in aaa.st17.sne22.ru.key -out aaa.st17.sne22.ru.key.nopass 
* sudo openssl rsa -in bbb.st17.sne22.ru.key -out bbb.st17.sne22.ru.key.nopass
```

<center>

![](https://i.imgur.com/npfcnIp.png =600x)
Figure 29: Using the Different Keys setup to disable
</center>

```
* sudo chmod 600 aaa.st17.sne22.ru.key
* sudo chmod 600 bbb.st17.sne22.ru.key
```
<center>

![](https://i.imgur.com/ZMJkJt8.png =600x)
Figure 30: Changing the mode of the key file
</center>

```
* sudo cp aaa.st17.sne22.ru.key /etc/ssl/private/
* sudo cp bbb.st17.sne22.ru.key /etc/ssl/private/
* sudo cp aaa.st17.sne22.ru.crt /etc/ssl/certs/
* sudo cp bbb.st17.sne22.ru.crt /etc/ssl/certs/
```
The final configuration for each of the domain configuration is given in the snapshot below as thus:

<center>

![](https://i.imgur.com/O6HoHCr.png =600x)
Figure 31: Server File for aaa Domain
</center>

The server file for the bbb domain is given in the snapshot as thus

<center>

![](https://i.imgur.com/MiWq0CG.png =600x)
Figure 32: Server File for bbb Domain
</center>

To validate the SSL/TLS of the domains
I logged into my browser to try out the domains and from the result it shows that i am using a self signed certificates with the response

**NB:** Although this could be frightening, based on the popped up windows, but I presume this is because the certificate used for this server is self-signed.

<center>

![](https://i.imgur.com/NR8iFpB.png =600x)
Figure 33: Result from Firefox browser
</center>

For aaa domain
<center>

![](https://i.imgur.com/HK1Y6DR.png =600x)
Figure 34: Certified aaa domain 
</center>

For the bbb domain to validate the SSL/TLS


<center>

![](https://i.imgur.com/J3BLNkG.png =600x)
Figure 35: Certified bbb domain
</center>

Using the curl command for the aaa domain

<center>

![](https://i.imgur.com/Cm9Gida.png =600x)
Figure 36: Curling the aaa domain
</center>


Using the Curl command for the bbb domain

<center>

![](https://i.imgur.com/yCFnVDF.png =600x)
Figure 37: Curling the bbb domain
</center>


---

## Bonus

:::info
3. Logging
Choose a log Analyzer, run some tests from various client locations and User-Agents, and produce statistics. Checking the logging information on your web server is important to discover problems and/or attacks on your web server. Define your own log format containing information you deem important. Use Conditional logging to add User-Agent and Referrer information to request logs that generated an error.
:::

Trying to create a logging system for the server I researched a tool which is GoAccess. This is a log analyzer tool and i want to build from the source

```
* wget https://tar.goaccess.io/goaccess-1.5.1.tar.gz
* tar -xzvf goaccess-1.5.1.tar.gz
* cd goaccess-1.5.1
* ./configure --enable-utf8 --enable-geoip=mmdb
* make 
* make install
```
We can also get the log software from the apt package manager

```
* sudo apt install goaccess
```
After running the install process of goaccess, we can check the log of the server by using the command that could aid that process

```
* goaccess /usr/local/nginx/logs/access.log --log-format=COMBINED
```

<center>

![](https://i.imgur.com/2dT5Dsv.png =600x)
Figure 38: Logs UI for generated traffic
</center>

I will also use this goaccess application to send command that would be in html format so i can read it from my browsers

This can be achieved by using the following command

```
* sudo goaccess /usr/local/nginx/logs/access.log -o /var/www/aaa.st17.sne22.ru/log_report.html --real-time-html
```
:::danger
On running the command i encountered a problem saying that i should check the time format, date format and log format of the configuration. 
:::

<center>

![](https://i.imgur.com/Bx84R9R.png =600x)
![](https://i.imgur.com/l9NdU9P.png =600x)
![](https://i.imgur.com/Hau4t1o.png =600x)

Figure 39: Error Message from Goaccess
</center>

:::success
**Solution**

To resolve this issue, i will check the configuration file of the goaccess since that is where the error points me to  and I would modify it.
<center>

![](https://i.imgur.com/WdtFmZ2.png =600x)
![](https://i.imgur.com/y9ExlLy.png =600x)
![](https://i.imgur.com/FkMyiDF.png =600x)
Figure 40: Modification of Error to Correct Problem
</center>

After the modification we can run the following command
```
* sudo goaccess /usr/local/nginx/logs/access.log -o /var/www/aaa.st17.sne22.ru/html/log_report.html --real-time-html
```
:::

After running this command, we would access the domain that carries this html page and access it from the  browser to get the real time feature as shown in the snapshot below

<center>

![](https://i.imgur.com/x32k5hG.png =600x)
Figure 41: Result of Logs UI for GoAccess
</center>

We could elaborate this feature by running login format from our configuration file of Nginx to get better output.

Thank you!

---
## Additional Features After Submission

:::info
### 1. Web Server Security
:bulb: Create a new virtual host and a HTML page with content (i.e Administrative area). Enable basic authentication in your web server for your new virtual host. Use password file creation utility and create two users. Verify your work by authenticating against your webpage.

:::

First thing to do regarding achieving something of this sort is to download a password generator utility. Although this procdure can be achieved in different ways but this a way can be beneficial. In this case i will an apache utility that would be good.

The command is given as 

```
* sudo apt install apache2-utils
```

<center>

![](https://i.imgur.com/lSHiLpm.png =600x)
Installed Utility Package
</center>

Having done that I would create some user using the following command. 

```
* sudo htpasswd -c /usr/local/nginx/.htpasswd user1
* cat /usr/local/nginx/.htpasswd
```

The second is used to verify if the user has been created. It is shown in the snapshot below

<center>

![](https://i.imgur.com/RaoNQ25.png =600x)
Created User
</center>

The next thing to do is to enable this process in the virtual host file that you have created. I will achieve this by editing the configuration of the subdomain i intend to use.
First before i achieved this, I migrated the file to the directory i would want it to be. in this case i moved the file using the command

```
* sudo mv /usr/local/nginx/.htpasswd /var/www/bbb.st17.sne22.ru/html/
```

Then after you have done that you could modify the configuration file of the subdomains

Following this command

```
* sudo nano /usr/local/nginx/conf/sites-available/bbb.st17.sne22.ru.conf
```

<center>

![](https://i.imgur.com/6uID6P9.png =600x)
Added configuration to the Script
</center>

To verify the configuration added I would try to access the webpage indicated.

<center>

![](https://i.imgur.com/JEKd3hh.png =600x)
Provide Credentials
</center>

After providing credentials, it would open this page that is directed to.

<center>

![](https://i.imgur.com/XBMrP6M.png =600x)
Successful Outcome
</center>

#### Application of These

This can be applied in your Nginx server whenever you are trying to restrict the access to a particular page in your web application. Although these are like basic extent you can go with securing your web infrastructure, but it gives a base to guide your thought process. 
You could also hardening it by choosing a **range of IPs to allow**, restricting access to the **htpasswd** file, hardening the encryption using the **ciphers values** and much more.

---
# Reference
1. [Building Nginx from Source](https://www.youtube.com/watch?v=R4hPcNAW33c&t=605s)
2. [Understanding Directories in Linux](https://askubuntu.com/questions/308045/differences-between-bin-sbin-usr-bin-usr-sbin-usr-local-bin-usr-local)
3. [Installing Nginx in Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04)
4. [HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
5. [SSL Configuration](https://www.nginx.com/blog/nginx-https-101-ssl-basics-getting-started/#SSL)
6. [Configuring Virtual Servers for Nginx](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/)
7. [Nginx Module](https://nginx.org/en/docs/http/ngx_http_core_module.html?&_ga=2.137077939.2001993380.1665603507-918983007.1662991122#server)
8. [SSL Temination](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/)
9. [Getting Started With GoAccess for Monitoring](https://goaccess.io/get-started)
10. [Apache Utility 2](https://installati.one/ubuntu/20.04/apache2-utils/#what-is-apache2-utils)
11. [Basic Authentication](https://tonyteaches.tech/basic-authentication/)
