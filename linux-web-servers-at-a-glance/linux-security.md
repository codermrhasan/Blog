# Linux Security

Security is more important as software development. Not only for developers but also for all the software users.

## Rule of Least Privilege

The user or an application has only permission to do it's job, nothing else. For example, to upgrade the os system you need to be a root user.

## Super User

Every linux machine has a super user called root. Root user can do anything on that system. Hackers already know that. To make more difficult to hack your server. It's your first job to create another user and use server with that user most of the time. When you need to root permission then use root command from this user. Root permit command starts with `sudo`. Then enter your actual command. presss enter. it will ask for password. Enter password and your command will run. Surprisingly you can't see your typed password.

## Sudo and Su

When you enter `sudo su` it will take your all command as root. You don't need to enter sudo every time. But stop. It's very risky aproach to access root command. Again, always give root command from other user with `sudo`.

## Package Source Lists

If you not used linux before you might be use some kind of store to install software. Linux also have store, but it's not so updated. It's better for security to stay up to date. So how to get softwares/packages in linux? All of your available package sources are listed in a file `sources.list`. To see what's inside the file enter `cat /etc/apt/sources.list`. Oveserve the file. Everything starts with `#` is comment. You can see some URLs. These are software repositories.

## Update and Upgrade

To update softwares first you need to update packages sources list by `sudo apt-get update`. Why sudo? Because this command needs root permission. This command will check what are available to update.  
The previous command didn't actually update any software. To update enter `sudo apt-get upgrade`. After few moments terminal will list which are going to change to newer. It will ask for permission. If it's the newly installed os you can type yes and enter. But when you already publish a web app on this machine, you need to carefull to upgrade. You should check everything in non-production environment first. If everything goes well then upgrade those specific things or whole system. Now enter `sudo apt-get autoremove` for removing packages that we don't need anymore.

## Finding Packages

Open up browser and go to packages.ubuntu.com and search for which package you need. Then install it by knowing the package name.

## Finger

Finger software help us to look up various info of a user and serve data as a easily readable way. Install finger by `sudo apt-get install finger`. Enter `finger` to see the currently logged in users to the system. Enter `finger < user >` to see more specific information of a user.

## Intro to /etc/passwd

Did you notice that how **finger** get these information? actually it collects data from /etc/passwd. Now observe this file by entering command `cat /etc/passwd`. Each line in this file is for a single user. Each line has some fields separated by colon(:). Now explore a line. For example;  
`rhp:x:1000:1000:Md Rakibul Hasan,,,:/home/rhp:/bin/bash`

- `rhp` - This is the first field. This is username of user
- `x` - This second field use to encrypt password. Now a days allmost every distribution use a character (x) to skip this
- `1000` - The third field is for user id
- `1000` - The fourth field is for group id
- `Md Rakibul Hasan,,,` - This fifth field is use to store better description of user. Sometimes it can be empty
- `/home/rhp` - The sixth field is users home directory
- `/bin/bash` - The seventh field is for users default shell

## User Management

We discussed before that using only root user is risky. Everybody know this. To secure it we have to create another use and disabling remote login to root user. If you use vagrant, it will automatically setted up. But some hosting provider may not set this up for you. Let's have a look how to do it manualy.

- `sudo adduser newuser` - This command will create a new user. Before creating user it will ask for some info. Like password, name, phone etc. Only password are required, others are optional.

- `ssh newuser@127.0.0.1 -p 2222` - `ssh` is for remotely connect to server. `127.0.0.1` is the ip that we want to connect. This ip is standard for localhost or our current machine. `student@` is the user we want to connected to that ip address. `-p 2222` tells us to connect using port 2222. After enter this command it will ask for permission and password.

- Now to give sudo permission to `newuser`, open up terminal with that user which have sudo permission. Most often the primary user. Enter `sudo cat /etc/sudoers`. In the end of this file you can see `#includedir /etc/sudoers.d`. This command tells the system to look also /etc/sudoers.d files and include those into this current file. This is standard to use this. Because if you add users directly in sudoers file, when system will upgrade you will lose all users you created. Now create and edit a file for edit by `sudo nano /etc/sudoers.d/newuser`. Add this to that file `newuser ALL=(ALL) NOPASSWD:ALL`. To save and exit press `ctrl x`. It will prompt for permission. Type y and press enter. Now open terminal which is loggedin with newuser. try to put sudo command. See you can do it.

- As an adminstrator if you want to force any of you users to change their password try to enter this command, `sudo passwd -e student`.

## Understand public key encryption
Suppose your friend has a special letter box and your friend only has it's private key. And public key is open for all. This key can only post a letter into this box. You can put any secret message to his letterbox and only your friend can watch it. Same way, Server send some random message to client. Client receive that, encrypt with their private key and send back to server. Server decrypt with the public key and checks everything is fine. 

## Key based authentication 

Key based authentication is more secure than password based authentication. Public key encryption

### Generating key pairs
Keep in mind, will generate our key pair on our local machine not on server. Never share private key with anyone else. Only share public key. That's why we need to genrate key pairs on local machine. we will use ssh-keygen to generate it. If it is not installed install it. Then enter `ssh-keygen`. It'll ask for filename first. give as recommended default direcoty. /Home/< username >/.ssh/< a name you like> . Then it will ask for passphrase for more security. If someone get these files this passphrase will prevent them to use those. Then it will generate /Home/< username >/.ssh/< a name you like> this is private key. And also generate /Home/< username >/.ssh/< a name you like>.pub this is public key. we will place this .pub file in our server to enable key based authentication. Other key types ssh-keygen supports with SSH protocol version 2; DSA, ECDSA, ED25519, RSA etc

### Installing public key
Open terminal and logged into `newuser`. First create directory .ssh by typing `mkdir .ssh`. In this directory all other key related file must be stored. Now command `touch .ssh/authorized_keys`. This file will store all other public keys that this account to user for authentication. must one key per line in this file. Now copy that local machines generated .pub files text into this authorized_keys file. Now set the permission for this file. Enter `chmod 700 .ssh` and then `chmod 644 .ssh/authorized_keys`. Later we will discuss file permissions.  

Now try to login newuser account with key. Enter `ssh newuser@127.0.0.1 -p 2222 -i ~/.ssh/<youe private key file name>`. It will ask for passphrase that you setted before. See you don't need to enter remote password to login.

### Forcing key based authentication
Final thing you want to do allow all users only key based authentication and removing password based authentication for more security. Open up your servers terminal and enter `sudo nano /etc/ssh/sshd_config`, then change PasswordAuthentication yes to 'no'. Now restart ssh by `sudo service ssh restart`. Now all user are forced to use key based authentication to access server.

### File Permissions
Go to any root directory and enter `ls -al`. you can see each line starts with some letters and (-) collection. For Example drwxr-xr-x, -rw------- etc. Before know that - is for file d is for directory. Now understand other nines. Actually this nine symbols/letters provide us three information. Let's explore drwxr-xr-x. There are three parts; rwx, r-x, r-x. The first part is for owner, 2nd part is for group and last is for everyone. The first part rwx means the owner can read, write and execute this file. The 2nd part r-x means groups can read, can not write, can execute this file. Last part r-x means everyone can read, can not write, can execute this file. There must be two columns which sometimes contains root root . The first column is for owner of this file and second columns is the group. By default when we create a newuser the a new group has create by same name of that user. You can modify this. But that is common practice.  

### Octal Permissions
Remember when we change file permission we use numbers instead of rwx. Lets learn the values r=4, w=2, x=1, no permission or (-)=0. Adding the three letter gives us permission for one set. And the combination of three sets gives us the full set of permission. Lets see an example; rw- r-- r--  means 6 4 4. So our full set of octal permission is 644. Now you can understand previous command `chmod 644 .ssh/authorized_keys`

### chgrp and chown
We see to change file/directories permission with chmod. If you want to change a files group use chgrp and if owner then use chown. To change group `sudo chgrp (group name) (file path)`. To change owner `sudo chown (group name) (file path)`.


## Ports 
A server can handling request of different types of requests like email response, web response, database response etc. Each application in server are configured to respond to requests for a specific port. We can control our server to accept specific requests throw specific ports by using firewall. Some common ports are;  
- HTTP - 80  
- SSH - 22  
- POP3 - 110  
- HTTPS - 443  
- FTP - 21  
- SMTP - 25  

## Firewalls 
We should not always response to all requests. We can configure this by firewalls. Ubuntu has ufw for firewalls. `sudo ufw status` you can see the status of firewall. If you are begginer, before configuring firewall make sure you firewall is inactive. After all setup open the firewall. The good practice is block every incoming requests and allow only which are need. `sudo ufw default deny incoming` deny all incoming requests. `sudo ufw default allow outgoing` allow all outgoing connections. If you enable firewall at these point, your server will deny all incoming requests, also you ssh connections. And your server will become permanantly unaccesible. 

## Configuring ports in ufw
First things first. We need to allow ssh. Normally you can enter `sudo ufw allow ssh`. But what if the port has changed. Suppose you use vagrant. Vagrant use port 2222/tcp. So enter `sudo ufw allow 2222/tcp`. To allow basic http server `sudo ufw allow www`. To enable firewall `sudo ufw enable`. Check what you have done `sudo ufw status`. 