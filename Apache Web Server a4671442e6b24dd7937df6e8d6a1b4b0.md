# Apache Web Server

[Miscellaneous Linux Commands](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Miscellaneous%20Linux%20Commands%20ee006cc90bb945dda6a06d3b80b3ad1d.md)

### Setting up Web server (**APACHE)**

Main configuration file of Apache is `httpd.conf`

`/etc/httpd/conf/httpd.conf`

**Root Document i.e. index.html is in**

`/var/www/html`

- /* USE of grep *
    
    Some grep options
    
    `grep ^abc` when we write this, then we mean that in the beginning of line `abc` string should be present
    
    `grep -B1 ^abc` **B1** here just means that show us 1 line **before** our result 
    
    Similarly, **A1** means show 1 line **after** our result
    
- firewall services
    
    [Chapter 5. Using Firewalls Red Hat Enterprise Linux 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-using_firewalls)
    
    ![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled.png)
    

### How to connect a client to a server

First disable SELinux, change from enforcing to disabled.

Make the changes given below into `ifcfg-ens33` file present in `/etc/sysconfig/network-scripts/`

- change bootproto from DHCP to static
- Make IPADDR=172.24.0.10 or any ip address that you want
- Make NETMASK=255.255.0.0
- Change network adapter settings from NAT to bridged
- **How do I rename ens33 to eth0?**
    
    To do so follow this steps: Edit /etc/default/grub. At the end of GRUB_CMDLINE_LINUX line append net. ifnames=0 biosdevname=0 Save the file. Type grub2-mkconfig -o /boot/grub2/grub. cfg Type reboot
    

### DHCP vs Static IP

Which is best for Trade Show Internet?

Whether your trade show setup requires big screens with cool videos or just a couple laptops. you’re going to need internet access.

The short answer is that DHCP is usually the best way to go for trade show internet needs. Here’s why, how IP addressing works, and when you might need static.

## What’s the difference between static and DHCP IP?

An IP address is a number that identifies each device on a network. With a static IP address, this unique number stays the same. With a DHCP (**Dynamic Host Configuration Protocol**) address, this number is autonatically assigned to each device from a pool of available numbers on the network. Just like it sounds: static is permanent; dynamic is temporary.

### There’s a reason it’s called an address.

The internet works a lot like a postal system. Just like street addresses identify where mail should be delivered, IP addresses identify where data should be delivered.

For example, let’s say you want to mail a letter. You’d write something like, “17 Cherry Tree Lane, Las Vegas, NV, 89123.” The address tells the post office that the letter’s destination is in the U.S.; the zip code identifies which city and state it’s in. (Fun fact: the post office largely ignores what you write as the city and state, only the humans looking at it really care. You could put “Disneyland, NV,” and as long as you have the right street address and zip, your letter would still arrive at its proper destination).

The letter then goes to the sorting center for that zip code. The sorting center narrows down the location by street address. It gets handed to your mail carrier and, voilà,“You’ve Got Mail!”

**************************/////////\\\\\\\\\***************************

This whole scenario corresponds similarly to the way the internet looks up addresses. That single house on Cherry Tree Lane would have a single (static) IP Address. For example, click on or type [35.238.194.115](https://hypernetworks.com/) or [54.244.12.90](http://54.244.12.90/) into a browser. Those are static addresses or IP addresses that don’t change. They resolve to hypernetworks.com and Disney.com respectively.

### When do you need a static IP?

If you want an IP address that doesn’t change for people in the outside world to get to your equipment, you need a static IP address. That makes static necessary for equipment that needs to be managed remotely, like servers and [VPN hardware](https://hypernetworks.com/2019/managed-services/vpn-issues-here-is-what-you-need-to-know/). Companies generally use static IP at their data centers and core infrastructure. Take a look at the chart below to get an idea of what needs static IP.

![https://hypernetworks.wpenginepowered.com/wp-content/uploads/2020/03/what-needs-a-static-IP-1024x495.png](https://hypernetworks.wpenginepowered.com/wp-content/uploads/2020/03/what-needs-a-static-IP-1024x495.png)

### Start Apache Web Server

When we start our system we use the command `systemctl start httpd`

The above command starts the server temporarily i.e. when we reboot our system we have to rerun the same command again

- What is httpd
    
    HTTP Daemon is a software program that runs in the background of a web server and waits for the incoming server requests. The daemon answers the request automatically and serves the hypertext and multimedia documents over the Internet using HTTP.
    

To make it permanent, we use `systemctl enable httpd` , this command permanantly enables the server i.e. when we boot into our system then we don’t have to start our server again and again.

`systemctl status httpd` : To check the status of the Apache server

### Why we use index.html as our home page and not something else like home.html? Can we use something like home.html??

When we see the content of `httpd.conf` file contained inside `/etc/httpd/conf/` we observe that the `DirectoryIndex` is named as `index.html`

![<IfModule> is simply **a directive that tests the condition "is the named module loaded by apache httpd"**](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%201.png)

<IfModule> is simply **a directive that tests the condition "is the named module loaded by apache httpd"**

If we want our homepage to be named as something else then change the name of `DirectoryIndex` to that name… For eg: home.html.

- What happens if we change name of our `index.html` file in `/var/www/html` to let’s say `i.html` and DirectoryIndex remains unchanged?
    
    In this situation when we will open our server’s ip address into on of our clients then The web server will not display the contents of `i.html` file instead it will display the contents of `/usr/share/httpd/noindex/index.html` file.
    

### Why, How and where? (default things)

### Default Welcome Page

It is a default index.html file which is loads when no index.html file is present. Located in `/etc/usr/httpd/noindex/`

### Change DirectoryIndex

inside httpd.conf file: write as follows

`DirectoryIndex index.html **i.html`** i.html is updated

here first Apache will look for index.html and then will look for i.html

after updating this run, 

`systemctl reload httpd` and not restart… as restarting will stop our server for sometime and then restart… So for that interval of time the client won’t be able to access the server.

then check with,

`service httpd configtest` whether your syntax is Okay or not

### Document Root

Sometimes we wonder why we wonder why we are putting data in `/var/www/html` directory. This is decided by one directive called `DocumentRoot`. 

### Change Default Port

By default, Apache Web Server is listing on port 80, that is decided by `Listen` Directive present in `httpd.conf` file in `/etc/httpd/conf` directory.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%202.png)

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%203.png)

Revert back to original (default) settings:

delete ServerName, i.html from DirectoryIndex, change port from 90 to 80… Also remove i.html file from /var/www/html/

Reload httpd

check syntax (`service httpd configtest`).

- **Virtual Hosting overview - Name Based vs IP based**
    
    Now, suppose if you have a Web hosting company and on your Web server, if you are going to launch only one site, then it is a wastage of resources.
    
    Now, what we want, we want to launch many sites on one Web server that is called virtual hosting, now…
    
    two types of virtual hosting are available.
    
    The first one is a name based virtual hosting.
    
    What’s the meaning of a **name based virtual hosting**? 
    
    It means that We want to **launch many sites by using just a single IP**.
    
    And please remember in real life, most of the hosting company that provide these services are based on this concept only i.e. name based.
    
    Because when we launch the real website, we need the real IP so if we have one real IP and We are going to launch many websites, then it will be cost effective for the customers.
    
    So mostly the offering that Web hosting give us They are based on that name based virtual hosting that mean in name based virtual hosting on single IP, We can launch many websites, 
    
    another type of hosting is **IP based virtual hosting** in which we can **launch one website on a single IP**.
    
    If you have to launch three sites, then you need three IPs.
    
    In the case of IP based virtual hosting, but in the name based virtual hosting, if suppose we have to launch three Web sites, we need only one single IP.
    

### Now we want to host 3 sites on a single IP… Let’s learn how to do this! ***NAME BASED VIRTUAL HOSTING***

![Lab setup](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%204.png)

Lab setup

For implementing name based virtual hosting, we need properly configured DNS server which will resolve “**www.example1.com**” “**www.example2.com**” “**www.example3.com**” to IP address “**172.24.0.1**”. Since we haven’t implemented **DNS server** at this point of time, we will implement the same logic by editing `/etc/hosts` files on all systems. 

![Make the same entry in both the clients as well.](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%205.png)

Make the same entry in both the clients as well.

### Edit httpd.conf file

We will add a directive `NameVirtualHost` and this directive will decide on which IP address, we are going to launch the three name based websites.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%206.png)

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%207.png)

check syntax(`apachectl configtest` `service httpd configtest` `httpd -t`)   and reload server.

### IP BASED VIRTUAL HOSTING

Lab:

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%208.png)

![Rest all will remain same.](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%209.png)

Rest all will remain same.

- After updating `hosts` file,
- also update `ifcfg-ens33` present in `/etc/sysconfig/network-scripts/` —> Update old & Add following Directives:

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2010.png)

- Create your index.html files in `/var/www/html/` in each directory example1 to 3
- Edit `httpd.conf` file and update VirtualHost IP Address.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2011.png)

Actually, what happens when we send a request like this `elinks —dump http://[www.example1.com](http://www.example1.com/)` then first it checks from the host file that 

what is ip address which is- 172.24.0.1

Then the request will be sent to Your Web server, web server will check which  offer that

to your website on 172.24.0.1.

So it will serve the material from that example1 directory.

When we hit any web address for example: [http://www.example1.com](http://www.example1.com) then 

1. First it checks from the host file what is the ip address `172.24.0.1` corresponding to this link.
2. Then request is send to the web server and it checks which sentence/command/directory offers that to your website on  172.24.0.1 
3. Apache will serve the material from example 1 directory.

### Host Based Security

Lab: 

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2012.png)

Now, we will be using one element which is called `Require`, it will be used for implementing host based security.

`Require` offers different ways to restrict access to your website or portion of website. Suppose there are different users such as admin, company, client and now we want to all of them to have different access to the website portion. To enforce different practices we can use `RequireAll`, `RequireAny`, `RequireNone`directives.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2013.png)

In the `httpd.conf` file present in /etc/httpd/conf, to deny access of a site from all or from any specific ip we’ll do as mentioned above in the picture.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2014.png)

accessible from everywhere except 172.24.0.10

### Apache User Authentication

Only authenticated users should be able to view our website.

Lab Setup:

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2015.png)

**Creating Apache Users**

### `**htpasswd**`

This command is used to create combination of usernames/passwords. 

The flag `-c` creates new file, if not already there and overwrites the file if already present there. The flag `-m` is used for `MD5` encryption. 

![4 users: Anantika, Vipin, Nanu and Kanu|](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2016.png)

4 users: Anantika, Vipin, Nanu and Kanu|

**After creating users, now let’s add require access in our webpage. Here we are doing this for example1 webpage.**

# Method 1

Edit httpd.conf

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2017.png)

[What is the difference between Digest and Basic Authentication?](https://stackoverflow.com/questions/9534602/what-is-the-difference-between-digest-and-basic-authentication)

Now, to access the website, just usually open the website through elinks, i.e.:

`elinks 172.24.0.1` or

`elinks —dump http://username:password@172.24.0.1`

# Method 2

instead of making edits to `httpd.conf` file, we can create a `.htaccess` file in `/var/www/html/example1` directory.

```bash
AuthType Basic
AuthName "Restricted Access"
AuthUserFile "/etc/httpd/webpass"
Require user user1 user2       
```

After creating .htaccess file, you have to change the option value of `"AllowOverride"` from “None” to “AuthConfig” in configuration file `httpd.conf`   

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2018.png)

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2019.png)

**To give access to all the valid(registered/authenticated) users:**

just update the value of `Require` from `user user1 user2` to `**valid-user**`

# Method 3

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2020.png)

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2021.png)

Removed .htaccess file at last of this module.

### Per-user Web Directories

**First we have to enable `“UserDir”` support for `"public_html"` directory based websites so that users can access the websites in `[http://172.24.0.1/~user](http://172.24.0.1/~user)` format.**

httpd.conf file was used to be a large file earlier and when we used to restart our webserver it first reads the httpd.conf file and then the files which are `included` in it with the help of`**Include`directive**. These files have an extension of `.conf`.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2022.png)

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2023.png)

- **What is Server Root?**
    
    The top of the directory tree under which the server’s configuration, error, and log files are kept.
    
    ServerRoot specifies where the subdirectories *conf* and *logs* can be found.
    
    If you start Apache with the `-f` (file) option, you need to include the `ServerRoot` directive. On the other hand, if you use the`-d`(directory) option, as we do, this directive is not needed.
    
- **htpasswd vs passwd**
    
    Passwd for unix like systems:
    
    ```bash
     https://en.wikipedia.org/wiki/Passwd
    ```
    
    Httpasswd for apache:
    
    ```bash
     https://httpd.apache.org/docs/2.4/programs/htpasswd.html
    ```
    

Now after above mentioned process, create users using `useradd` command and set their password using `passwd` command.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2024.png)

After logging in as user and doing the above mentioned process… 

Change the permission of public_html directory to 755 `(chmod 755 public_html)`.

Run the website i.e. on your c10 system run `elinks --dump 172.24.0.1/~user1`.

This will show the following error:

![I have made other users beside nanu and vipin, namely user1, user2, user3 and user4.](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2025.png)

I have made other users beside nanu and vipin, namely user1, user2, user3 and user4.

This error is due the existing permissions set for this file.

Using `**chmod`** change the permission of the home directory of **user1** as well to **711.**

Now, rerun your website on c10 system and boom you are done!

### Checking Log-files for Troubleshooting puposes

There are 2 log files:

1. access_log at `/var/log/httpd/**access_log**`
2. error_log at `/var/log/httpd/**error_log**`

Both the log files are maintained in **common log format** which is standard text log format which can be analyzed by many analyzers.

<aside>
🔗 [https://www.sumologic.com/blog/apache-logs-vs-nginx-logs/](https://www.sumologic.com/blog/apache-logs-vs-nginx-logs/)

</aside>

### Indexes

Sometimes when we access a ip address instead of webpage we get displayed an index of directories and files in it.

To enable and disable it:

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2026.png)

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2027.png)

### Redirect

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2028.png)

### LAMP Stack

L: linux A: Apache M: Mariadb P: PHP

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2029.png)

Now, inside `/var/www/html` create an `index.php` file.

**Why index.php?**

Ans: Same reason as of index.html i.e. The `DirectoryIndex` directive that decides which files will be served in what order, to the web clients, The `index.php` is included in the DirectoryIndex directive.

Configure an ip for index.php

### Starting Mariadb

check whether mariadb is installed or not using `rpm -q mariadb-server` & `rpm -q mariadb`

start the mariadb server using `systemctl start mariadb` to start it and 

`systemctl enable mariadb` to permanently start mariadb.

### Connnecting to mariadb server

Use the command `mysql` to open the database; terminal will display as shown…

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2030.png)

### Creating Database and user

![I have created ‘rahbar’ as databse and ‘ignis’ as user with pass ‘ignisiitk’.](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2031.png)

I have created ‘rahbar’ as databse and ‘ignis’ as user with pass ‘ignisiitk’.

Using mysql create a table and make some entries.

![the code shown below will create a table and we can insert values manually as shown in the above image.](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2032.png)

the code shown below will create a table and we can insert values manually as shown in the above image.

```sql
create table salary (
	employee_no var(3),
	department varchar(15),
	amount int(6)
);
```

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2033.png)

Now just run `elinks --dump http://172.24.0.1`

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2034.png)

### Use HTTPS

for using https we need to configure to files namely `httpd.conf` and `**ssl.conf**` , the later is present in `/etc/httpd/**conf.d**` directory. 

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2035.png)

We are interested in **SSLCertificate**… This directive is present in ssl.conf file.

![By default, `/etc/httpd/conf.d/ssl.conf` contains information certificate file `localhost.crt` and private key file `localhost.key`](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2036.png)

By default, `/etc/httpd/conf.d/ssl.conf` contains information certificate file `localhost.crt` and private key file `localhost.key`

### Create own private key

`openssl genrsa -out master.key 1024`

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2037.png)

### Create self signed certificate

`openssl req -new -key master.key -out master.crt -x509`

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2038.png)

- What is x509?
    
    In cryptography, X.509 is an International Telecommunication Union standard defining the format of public key certificates. X.509 certificates are used in many Internet protocols, including TLS/SSL, which is the basis for HTTPS, the secure protocol for browsing the web
    
    An X.509 certificate is a digital certificate based on the widely accepted [International Telecommunications Union (ITU) X.509 standard](https://www.itu.int/rec/T-REC-X.509), which defines the format of public key infrastructure (PKI) certificates. They are used to manage identity and security in internet communications and computer networking. They are unobtrusive and ubiquitous, and we encounter them every day when using websites, mobile apps, online documents, and connected devices.
    

**These certificates and private key are stored in certain directories which are as follows:**

for certificate → `/etc/pki/tls/certs`

for private key → `/etc/pki/tls/private`

### Modify ssl.conf

Update the `SSLCertificate**File`** directive and `SSLCertificate**KeyFile**`directive.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2039.png)

Now restart httpd

### Verify https access

`openssl s_client -connect [www.example.com:443](http://www.example.com:443) | head -6`

https works on port 443, normal webserver works on port 80, which is default so need to mention port 80.

**Now to use our browsers i.e. curl or elinks:**

For **curl** simply use `-k` with curl it will override the SSLCertificate error. This error is due to the SSL Certificate that we have made and is not made by any renowned authority or verified authority.

For **elinks:** update the value of `connection.ssl.cert_verify` from 1 to 0 in `/etc/elinks.conf` file.

![Untitled](Apache%20Web%20Server%20a4671442e6b24dd7937df6e8d6a1b4b0/Untitled%2040.png)