# Linux at a glance 

Do you know what happen when you visit a website in your browser? How do the page shown up? Basically when you type and enter a link in your browser, your browser send request to the **server** of that specific website. Then that **server** return the requested page and you can watch it. So what is **server**? Simply a **server** is nothing but a computer which is connected to the web and it is always on because of response with our request. Maximum number of server computer linux os. So It's important to understand it.

## Linux Operating Systems
There are many varities of linux os. These are always free to use. Also have paid versions. Some arguing people will told you linux is bad because it has not suitable for enterprises. That's absolutely wrong. One example is enough for them, that's redhat linux os. Some popular linux os are;
* RedHat - Better for enterprise/corporate customers
* Ubuntu - Easy to use on servers, desktop, laptops
* CoreOS - Better for containerized deployment of apps
* Debian - It's the core OS of other debian based OS
* Kali   - Better for security and hacking

## How hosting computer and linux are connected?
Mostly your hosting provider gives you a virtual machine. You log into it via SSH. Then you have to install your prefered OS. You can also set it up with you personal computer for testing perpose. The benefit of VM is, what you do inside vm's os, has no impact on core OS.

## Basic commands for interacting with files and directories
```pwd``` - It' will show you in which directory you are in
```ls```  - List of files and directories in this directory
```ls -al``` - List of all files and directories in this directory (including hidden files and directories)
```cd``` - Takes you to the /home/< user > directory
```cd < directory path >``` - Takes you to the directory that you provided
```cd ..``` - Go one level up to the directory
more commands you will get in later tutorial.

## Understanding important direcotries
Open up terminal. Enter ```pwd```. You can see you are in /home/< user > directory. By default terminal will open in this directory. If you type ```ls -al```. You can see, something like these;
```-rw-r--r--   1 < user >  < user >   4979 Dec  9 20:31 .bashrc```
```drwxr-xr-x   2 < user >  < user >   4096 Dec 12 23:43 Desktop```
If the first character is ```d``` thats a directory, if it's ```-``` that's a file. We will learn all other signs in later tutorial.

Move into the root by ```cd /```. Now enter ```ls -al``` to list all directories. 
home - This is default directory.
etc - Is for configuration files. (Ex: As we start setting up database and web server, we have to modify some files here)
var - Is for variable files. 
bin - Is for executable binaries those are accesible by all users.
sbin - Similar to bin except it's for root user.
lib - It's for libraries that support binaries that are locate around the system.
usr - For user programs. Most like as binaries in bin. except binaries in bin required bootup and binaries in usr don't need.

## linux $PATH
How does the `ls` command work? Actually it's a binary program. So where this program located? Move up to /bin directory and list all files. You can see there is a file named ls. When we enter ls command then our computer actually run this file. If type give this `ls` command from anywhere of your computer it will work. So how does your computer know that `ls` commands refer to `/bin/ls`? That is $PATH variable. The path variable contains some directory/file path to make that files/file available through whole the computer or specific to some place.