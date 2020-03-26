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

## Add grader user
- You can add a new root user such as 'grader' by entering the following command in your linux terminal via ssh: 'sudo adduser grader'


## Ackowledgement:

I'd like to acknowledge Udacity as most of the processes I followed are based on the lectures. 
I also referenced the work of the following people, they were very detailed and helpful:
- https://medium.com/@zionoyemade/deploying-flask-application-to-amazon-lightsail-199e79bb256a
- https://github.com/samrat-b/linux-server-config
- https://github.com/chuanqin3/udacity-linux-configuration
- https://github.com/Justin-Tadlock/linux-server-config-project

And also the friends and mentors who were always eager to help and support!

  
  
