# Project 1 - LAMP Stack Deployment
**Step 1 - Installing Apache & Updating the firewall**
---

- Ran `sudo apt update` & `sudo apt install apache2` to update the packages on the server and install apache.

  - Also ran `sudo system ctl status apache2` to verify apache was running in my linux server. See screenshot below.
![apache running](https://user-images.githubusercontent.com/91850543/158664449-b6297870-3913-4d00-8bc7-2373f8686da1.png)

- Modified the network security group from the AWS console to allow inbound traffic to port 80 for HTTP connections. See below screenshot.
![inbound rules](https://user-images.githubusercontent.com/91850543/158665491-c6b59677-21cf-4083-b586-5705ace372e8.png)

- Ran a curl to check if apache server is reachable. Used `curl http:localhost:80` & `curl http:127.0.0.1:80`. See output below.
![curling to check http](https://user-images.githubusercontent.com/91850543/158665855-8ca3f568-7103-41fa-8d56-771aea2e2e1e.png)

- Ran `http://54.90.168.4:80` to check if apache was reachable on a browser. see the result below.
![image](https://user-images.githubusercontent.com/91850543/158668702-2e131040-89c5-4e23-a50b-b8ed21fec628.png)


**Step 2 - Installing MySQL**
---

- Ran `sudo apt install mysql-server` to install MySQL server on server. Also ran `sudo mysql_secure_installation` for secure installation (password). Provided a password of MEDIUM strength and saved.
  - Ran `sudo mysql` to check if I'm able to log into MySQL. See result below.
![mysql login](https://user-images.githubusercontent.com/91850543/158676582-3da86263-688b-457e-8ed0-d7e09a8c71f7.png)

**Step 3 - Installing PHP**
---

- Ran `sudo apt install php libapache2-mod-php php-mysql` to install MySQL on my linux server.

  - After the installation completed, I ran `php -v` to confirm the installed php version. See output below. (PHP version 7.4.3 confirmed)
![php running](https://user-images.githubusercontent.com/91850543/158677874-079b12af-5f69-4215-ab4b-a585e3eb93f4.png)

LAMP stack completely installed and fully operational.

**Step 4 - Creating a virtual host for your website using Apache**
---

- Ran `sudo mkdir /var/www/projectlamp` to create a directory for projectlamp. Used cd to locate directory. See result in below screenshot.
![image](https://user-images.githubusercontent.com/91850543/158682857-9b480cca-30bb-4bcd-a715-e49a53a9f383.png)

- Ran `sudo chown -R $USER:$USER /var/www/projectlamp` to assign directory ownership to current system user.
![image](https://user-images.githubusercontent.com/91850543/158686542-1bc3b9e8-2704-4134-8c0d-677fdc6ef1ab.png)

- Ran `sudo vi /etc/apache2/sites-available/projectlamp.conf` to create the projectlamp.conf file. Pasted in the bare-bones config files and saved by pressing `esc` to exit edit mode and `:wq` to write to the config file and quit, saving it upon exit. See result below.
![image](https://user-images.githubusercontent.com/91850543/158684784-d0e5c6a3-cda7-45f5-8791-632aa25faa61.png)

- Ran `sudo ls /etc/apache2/sites-available` to display the new file in the sites available directory. See output below.
![sites available](https://user-images.githubusercontent.com/91850543/158685554-21095007-2503-442e-9b23-6a383d6dd8a6.png)

- Ran `sudo a2ensite projectlamp` to enable the new virtual host. Also ran `sudo a2dissite 000-default` to disable apache's default website. See screenshot below.
![image](https://user-images.githubusercontent.com/91850543/158686950-899941d8-542b-4883-b888-e287be135152.png)

- Ran `sudo apache2ctl configtest` to ensure my config doesn't have any syntax errors. Also ran `sudo systemctl reload apache2` to reload apache so the changes take effect. See below.
![image](https://user-images.githubusercontent.com/91850543/158687222-b900a708-3160-4061-80af-5e9435cf88cb.png)

- Created an index.html file in /var/www/projectlamp to check if the virtual host works as expected.

  - Ran `sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html` and went to browser to check if echo command was successful. See result below.
![lamp html](https://user-images.githubusercontent.com/91850543/158688448-7db78bfb-812b-4ffc-819b-933709f6b5ce.png)

**Step 5 - Enable PHP on the website**
---

- By default on apache, an index.html file will **always** take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
  - To change this, we need to modify the /etc/apache2/mods-enabled/dir.conf file by running `sudo vim /etc/apache2/mods-enabled/dir.conf`.

  ```
  <IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
  </IfModule>
  ```

  - After making changes, use `sudo systemctl reload apache2` to reload to activate changes.

- Create a new index.php file inside custom web root folder. See screenshot below.
![sudo vim](https://user-images.githubusercontent.com/91850543/158693615-bb073eac-ad5b-4b34-b020-7aa02e19f6b4.png)

  - Pasted below php code inside index.php file

```
  <?php
phpinfo();
```
  - Saved the file with `:wq`.

- Reloaded browser page and got the PHP page below.
![lamp php](https://user-images.githubusercontent.com/91850543/158694449-16502804-b913-472f-a4e2-88b6e965e80b.png)

Project 1 Complete. LAMP stack successfully deployed!





