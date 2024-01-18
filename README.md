# strapi-deploy-steps-for-digitalocean

To deploy a Strapi application on a DigitalOcean Ubuntu server with Apache2, you'll need to follow these steps:

1. Provision a DigitalOcean Ubuntu server:
   - Create a droplet on DigitalOcean with Ubuntu as the operating system.
   - SSH into your server using a terminal or SSH client.

2. Install Node.js and npm:
   - Update the package index on your server:
     ```
     sudo apt update
     ```

   - Install Node.js and npm:
     ```
     sudo apt install nodejs npm
     ```

3. Install and configure Apache2:
   - Install Apache2:
     ```
     sudo apt install apache2
     ```

   - Enable Apache2 modules:
     ```
     sudo a2enmod rewrite proxy proxy_http proxy_balancer lbmethod_byrequests headers
     ```

   - Restart Apache2 to apply the changes:
     ```
     sudo systemctl restart apache2
     ```

4. Configure a virtual host for your Strapi application:
   - Create a new Apache2 configuration file for your Strapi app:
     ```
     sudo nano /etc/apache2/sites-available/your-app.conf
     ```

   - Add the following configuration to the file, replacing `your-app` with your desired domain name or IP address:
     ```apache
     <VirtualHost *:80>
       ServerName your-app

       ProxyPreserveHost On
       ProxyPass / http://localhost:1337/
       ProxyPassReverse / http://localhost:1337/

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
     </VirtualHost>
     ```

   - Save and close the file.

   - Enable the virtual host:
     ```
     sudo a2ensite your-app.conf
     ```

   - Restart Apache2:
     ```
     sudo systemctl restart apache2
     ```

5. Install and configure PM2 for process management:
   - Install PM2 globally:
     ```
     sudo npm install -g pm2
     ```

   - Navigate to your Strapi project's root directory:
     ```
     cd /path/to/your-strapi-project
     ```

   - Start your Strapi app with PM2:
     ```
     pm2 start npm --name "your-app" -- start
     ```

   - Save the PM2 process list:
     ```
     pm2 save
     ```

   - Enable PM2 to start your app automatically on system boot:
     ```
     pm2 startup
     ```

6. Configure your domain name or DNS:
   - If you're using a domain name, make sure you have an A record pointing to your server's IP address.

7. Test your Strapi application:
   - Access your Strapi app by visiting your domain name or server IP address in a web browser.

That's it! Your Strapi application should now be deployed on your DigitalOcean Ubuntu server using Apache2. Make sure to secure your server, set up SSL/TLS if needed, and consider using a reverse proxy for added security and performance.
