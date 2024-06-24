# Setting up an SFTP Server on Ubuntu
## Prerequisites
Before starting, ensure you have:
A machine running Ubuntu (version 18.04 or later is recommended for the best compatibility)
SSH installed on your Ubuntu machine. This is usually pre-installed on most Ubuntu distributions.
Sudo privileges or root access to the Ubuntu machine

## Step 1: Create a New User
Open your terminal and type the following command to add a new user. Replace ‘sftpuser’ with the username you want to use.
```bash
sudo adduser sftpuser
```
## Step 2: Install SSH (if not already installed)
If SSH is not already installed on your server, you can install it by running the following command:
```bash
sudo apt-get update 
sudo apt-get install openssh-server
sudo systemctl status ssh 
```
## Step 3: Configuring SSH for SFTP
Now, we need to edit the SSH configuration file to specify the SFTP settings. We will use nano editor for this, but you can use vim or any other text editor you are comfortable with.
```bash
sudo nano /etc/ssh/sshd_config
```
Scroll to the very bottom of the file and add the following lines:
```bash

Match User sftpuser
    ForceCommand internal-sftp
    PasswordAuthentication yes
    ChrootDirectory /home/sftpuser
    PermitTunnel no
    AllowAgentForwarding no
    AllowTcpForwarding no
    X11Forwarding no
```
In these settings:
‘Match User’ specifies that the following lines only apply to the ‘sftpuser’ user.
‘ForceCommand internal-sftp’ restricts the user to SFTP only and disallows SSH.
‘PasswordAuthentication yes’ allows password authentication for this user.
‘ChrootDirectory /home/sftpuser’ sets the user’s home directory as their root directory, preventing them from accessing other parts of the server’s file system.
The last four lines disable various SSH features to improve security.
Once you’ve made these changes, save and exit the editor. If you’re using nano, you can do this by pressing ‘Ctrl + X’, then ‘Y’, then ‘Enter’.

## Step 4: Restart the SSH Service
After making the changes, restart the SSH service for them to take effect.
```bash
sudo systemctl restart ssh
```

## Step 5: Testing the SFTP Connection
Now it’s time to test the SFTP connection. From your local machine, attempt to connect to your server using the ‘sftp’ command, replacing ‘your_server_ip’ with your server’s IP address.
```bash
sftp sftpuser@your_server_ip
```
You’ll be prompted to enter your password. If everything is configured correctly, you should now be connected to your SFTP server.

## Step 6: Permissions and Ownership
After setting up the SFTP server, it’s important to check and manage the ownership and permissions of the user’s directory. The ChrootDirectory (in our case, /home/sftpuser) should be owned by root and should not be writable by any other user or group. This is a requirement of the SFTP setup.

First, change the ownership of the directory to root:
```bash
sudo chown root:root /home/sftpuser
```
Next, set the permissions for this directory. This command removes write permissions for group and other users:
```bash
sudo chmod 755 /home/sftpuser 
```
Please note that the sftpuser will not be able to write in the root of their home directory because it is owned by root. To allow the sftpuser to upload files, we need to create a directory inside the home directory that sftpuser owns.
```bash
sudo mkdir /home/sftpuser/files 
sudo chown sftpuser:sftpuser /home/sftpuser/files
```

Now, sftpuser can upload files to the /files directory.

## Step 7: Final Testing
Let’s perform a final test to make sure everything is working as expected. Try to connect again from your local machine:
```bash
sftp sftpuser@your_server_ip
```
Once you’re logged in, navigate to the files directory and try to create a new file:
```bash
cd files 
touch test.txt
```
## If the file is created without errors, this means that you have successfully set up your SFTP server and the user permissions are correct. Don’t forget to exit the SFTP shell:

 
# Conclusion
In this process of setting up an SFTP server on Ubuntu. We’ve created a new user, installed and configured SSH for SFTP, and set the correct permissions and ownership for our user’s directory. Now we can securely transfer files to and from your Ubuntu server using SFTP. It’s important to remember that while SFTP is secure, we should always follow best practices for managing your server, like regularly updating your software and using strong, unique passwords for all your users.
