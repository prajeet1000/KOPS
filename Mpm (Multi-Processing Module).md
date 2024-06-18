# Increase Apache Requests Per Second by installing mpm modules
## How Many Requests can apache2 handle per second
by default, Apache2 can handle up to 160 requests per second

## steps to increase Apache Requests per Second
1. Install MPM Module
   install mpm module in apache with following command
   ```bash
   a2dismod mpm_prefork
   a2enmod mpm_event
   apachectl -M | grep 'mpm'
   ```
2. Increase Max Connections in Apache
   open MPM Configuration file:
   ```bash
   vi /etc/apache2/mod-available/mpm_worker.conf
   ```
   You will see the following lines
   ```bash
   ServerLimit 250
   StartServers 10
   MinSpareThreads 75
   MaxSpareThreads 250
   ThreadLimit 64
   ThreadsPerChild 32
   MaxRequestWorkers 6000
   MaxConnectionsPerChild 10000
   ```
   You can change them to the following configuration to increase max requests per second. the Following configuration 
   supports up to 6000 concurrent users
   
• Serverlimit – Maximum number of Apache processes
• StartServers – Number of processes to start when you start running Apache
• MinSpareThreads/MaxSpareThreads – Number of threads to keep idle without being killed
• ThreadsPerChild – Number of threads per process
• MaxRequestWorkers – Number of concurrent connections to be supported. This is the main directive that you need to change to increase max connections in Apache
• MaxConnectionsPerChild – Number of connections to be handled by each child before it is killed

3. Restart Apache Server
   Restart apache web server to apply changes.
   ```bash
   service apache2 restart
   ```
