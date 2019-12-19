# Python Deployment Basic

You can learn here how to deploy python app with apache http server, mod_wsgi and ubuntu.

## Making your normal pc as a hosting pc

At this time you are learning. May be don't want to spend money to buy hosting and mess it up. That's why It's time to think alternative. Change your pc as a hosting pc. Most of of the hosting providers gives you a virtual machine with os installed. We use same in our pc.

## Virtual Machine

Virtual machine is a software that helps you to install different kinds of operating systems in your main os. Any modification in virtual machine has no impact on main os. We use Vagrant.

## Vagrant

Before installing vagrant make sure you enabled virtualization in bios setting. I guess that your main os is ubuntu. So I give you the instruction for this os. For other os you can goole it.

- Install VirtualBox by `sudo apt-get install virtualbox`. Make sure it is installed by `vagrant --version`. It will show the vagrant version.
- Create a demo project directory. Open terminal in that directory. Enter command `vagrant init ubuntu/bionic64`
- Now this step is not for real world hosting. It's for your pc. Open Vagrantfile and find `# config.vm.network "forwarded_port", guest: 80, host: 8080`. This line is commented by #. Just uncomment it. Save it and enter `vagrant up`. If any port related error shows up change `host: 8080` to what ever port you want. I have changed it to `host: 8765`. Now enter vagrant up. And you are ready.  
  Some common vagrant command;  
- `vagrant up` - if os has not downloaded previously it download that and start running.  
- `vagrant reload` - reload your virtual machine
- `vagrant status` - shows you vagrant is running or not.  
- `vagrant suspend` - sleep vagrant as like your pc sleep.  
- `vagrant ssh` - log you into your virtual os. You got the terminal access of your virtual os.  
- `vagrant halt` - all work is saved and virtual os turned of like your pc turned of.  
- `vagrant destroy` - destroy your virtual machine like formatting hard drive of a pc.  

## Apache http server
Now we should install apache http server as it is so popular. 
- Open up your vagrant virtual machine in that terminal enter command `sudo apt-get install apache2`. After installing apache visit http://localhost:8080 or which port you have saved. I have changed it to 8765 so for mine it would be http://localhost:8765. If you can see **Apache2 Ubuntu Default Page** it works. If not recheck installations or google it. Apache by default serves its files from /var/www/html directory. It has an `index.html` you can change it and can see the change by visiting http://localhost:8080.  


## mod_wsgi 
This is for python dependand web app.  
- Install mod_wsgi by `sudo apt-get install libapache2-mod-wsgi`
- Edit /etc/apache2/sites-enabled/000-default.conf. Before </VirtualHost> line write `WSGIScriptAlias / /var/www/html/myapp.wsgi` and save.
- Restart apache by `sudo apache2ctl restart`

You might get a warning "Could not reliably determine the server's fully qualified domain name". Visit https://askubuntu.com/questions/256013/apache-error-could-not-reliably-determine-the-servers-fully-qualified-domain-n to solve this.  

## WSGI Application
Most of the python web framework use WSGI. Like Flask and Django. But here we quickly test if everything goes fine. Earlier you have changed apache conf file. That's why create `/var/www/html/myapp.wsgi` file and edit it like bellow;  
`python
def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'

    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
`  
Save it and recheck the link http://localhost:8080. Boom you can see a 'Hello World!' in your browser. 