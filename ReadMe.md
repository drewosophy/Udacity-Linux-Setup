# Linux Server Setup

This Linux server setup goes through the process of setting up a Linux instance on AWS Lightsail, and amongst other things, configuring firewalls and giving sudo access to another user called grader according to the Udacity Course Requirement. I did the following:

- Made the following changes to the server:
- Switched the ssh port to 2200
- Blocked other ports apart from 2200 and 80.
- Created a new user called 'grader' and granted sudo access

## Note

i. IP address: 3.10.223.58/, port: 2200

ii. http://3.10.223.58/

## AWS Linux Server Setup
- Create an Amazon AWS account
- Navigate to the Lightsail product and set it up
- Create a Linux - Ubuntu instance
- Connect to the Instance using the Amazon ssh (alternatively you can ssh using your local terminal, but I found this more straightforward during the initial setup)

## Managing server dependencies
Start by updating the installed packages using 'sudo apt-get update' and 'sudo apt-get upgrade. Using sudo apt-get, you can install the required dependencies. I can't remember them all but I installed the following:
      git, 
      postgresql, 
      nginxgunicorn,
      flask,
      flask_httpauth,
      sqlalchemy,
      httplib2,
      requests,
      google-auth,
      psycopg2-binarybut 
      
If you run into any errors based on your setup, another good starting point is [Justin Tadlock's repo](https://github.com/Justin-Tadlock/linux-server-config-project) in terms of server applications and python3 configurations.  

## Connecting from your local machine using ssh
- Go to the account page, navigate to SSH keys and download the private key. This should be a .pem file format.  
- Then go to Networking and add the required ports according, 123 and 2200. Don't just do this from the terminal directly on your Linux instance alone, I faced issues at first. AWS can be painful in this area so set it up in the Networking UI as well. You can update these changes on your instance by editing the 'sshd_config file' using sudo nano /etc/ssh/sshd_config.  
- There are a couple of ways to get the key running on your local machine. 
- If you're a little more confident, navigate to your root directory on finder and find the .ssh folder. You can use 'ls -a' to find hidden files. Then use mv to move your downloaded key to the .ssh folder.
- Or you can make a new directory if you're struggling with the above using 'mkdir .ssh'. Then create an authentication key file using 'touch .ssh/replace with your new file name'. You can then edit the file from terminal using 'nano .ssh/replace with your new file name'. Copy the content from the .pem file you downloaded and voila!
- You can then ssh into your terminal. I will demonstrate my catalogue project IP address: 'ssh ubuntu@3.10.223.58/ -p 22 -i ~/.ssh/replace with your ssh key file name'. 
- You should now be connected from your local machine. 

## Setting up grader user and server for deployment
- You can add a new root user such as 'grader' by entering the following command in your linux terminal via ssh: 'sudo adduser grader'
- Refresh the terminal and generate a key for grader ssh access: 'ssh-keygen -f ~/.ssh/udacity_key.rsa'
- Read and copy the public key: 'cat ~/.ssh/udacity_key.rsa.pub'
- Navigate to the grader folder using 'cd /home/grader'
- Create a new .ssh directory similar to the one on your local machine using 'sudo mkdir .ssh'
- Create a file to save the keys 'sudo touch ssh/authorized_keys'
- To edit the keys in your terminal, use 'sudo nano .ssh/authorized_keys', paste the keys in and you're done with the keys setup!
- Then you can change the root user to 'grader' using the following: 'sudo chown -R grader:grader /home/grader/.ssh'
- Exit and reboot the server instance.
- Using the key for grader, follow the earlier instruction and ssh into the instance using the IP grader@3.10.223.58.
- To disable root login and make sure we use key-based authentication, navigate to your sshd_config end edit both in 'sudo nano /etc/ssh/sshd_config'.  
- Log into server as grader using the key file path you created 'ssh -i ~/.ssh/grader_key.rsa grader@3.10.223.58'.

## Configure UFW (Uncomplicated Firewall)
- sudo ufw allow 2200/tcp
- sudo ufw allow 80/tcp
- sudo ufw allow 123/udp
- sudo ufw enable
- Reboot the server

## Deploy the Application
- ssh in as grader again
- Navigate to 'cd /var/www' 
- Make a new directory called 'Catalog' 
- Clone the catalog repo using git clone (mine is https://github.com/drewosophy/course_catalogue/) into the directory.
- Create and edit wsgi file 'sudo touch catalog.wsgi' and 'sudo nano catalog.wsgi'
- Copy the following into the terminal:

```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application
application.secret_key = 'supersecretkey' 

```

- Rename the application.py to __init__.py
- Get the virtual environment running: 

```

sudo apt-get install python-pip
sudo pip install virtualenv
sudo virtualenv venv
source venv/bin/activate
sudo chmod -R 777 venv

```

- Edit your new __init__.py file using 'nano __init__.py' and change the 'client_secrets.jso' filepath in your CLIENT_ID variable to to '/var/www/catalog/catalog/client_secrets.json'.
- Adjust your host and port addresses. 
- Configure and Enable your virtual host
- Setup your database with a new user and adjust your db engine code to creating a PostgreSQL engine, adjusting the destination according to your setup.
- Using python3, run the python files to create the DB and load the default data into it. 
- Restart the server and you should be able to see your live application

This process is not entirely straightforward, I found myself encountering various issues along the way so just keep trying and check out some of the links in my acknowledgements!

## Ackowledgement:

I'd like to acknowledge Udacity as most of the processes I followed are based on the lectures. 
I also referenced the work of the following people, they were very detailed and helpful:
- https://medium.com/@zionoyemade/deploying-flask-application-to-amazon-lightsail-199e79bb256a
- https://github.com/samrat-b/linux-server-config
- https://github.com/chuanqin3/udacity-linux-configuration
- https://github.com/Justin-Tadlock/linux-server-config-project

And also the friends and mentors who were always eager to help and support!

  
  
