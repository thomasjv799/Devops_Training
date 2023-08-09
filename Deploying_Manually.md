# Deploying a Sample Node.js Program on EC2 with Apache2

This guide provides step-by-step instructions to deploy a Node.js application on an EC2 instance using Apache2 as the web server.

## 1. Update Packages and Libraries

```bash
sudo apt update
```

Updating packages and libraries ensures that your system's software repositories have the latest information about available
software packages. It's important to start with updated information to ensure compatibility and security.

## 2. Add Node.js 16 PPA

```bash
curl -sL https://deb.nodesource.com/setup_16.x | sudo bash -
```
This step adds the Node.js 16 PPA (Personal Package Archive) to your system. PPAs provide access to software that isn't available in the default repositories.
This command fetches the script from NodeSource, which sets up the PPA for Node.js 16.

## 3. Update Packages Again

```bash
sudo apt update
```
Updating packages again ensures that the newly added Node.js PPA information is incorporated into the package lists

## 4. Install Node.js and Verify Versions

```bash
sudo apt install nodejs
node --version
npm --version
```

Installing Node.js provides you with the Node.js runtime environment. After installation, you verify the
installed versions of Node.js and npm (Node Package Manager) to confirm that the installation was successful.

## 5. Update NPM Version

```bash
sudo npm install -g npm@9.7.1
```

This command updates the npm (Node Package Manager) tool to version 9.7.1 globally. Updating npm ensures that 
you're using a version that's compatible with your project and other dependencies.

## 6. Generate SSH Keys
Generate SSH keys for cloning and pushing code:

```bash
ssh-keygen
```
Generating SSH keys allows you to securely connect to other servers and services, like GitHub, for code cloning and pushing. 
SSH keys provide a secure way to authenticate without needing to enter your password every time.

## 7. Clone the Code
Clone your Node.js application code using SSH keys:

```bash
git clone git@github.com:yourusername/your-node-app.git
```

Cloning your Node.js application's code using SSH keys allows you to obtain the project's source code from a
remote repository (like GitHub) onto your EC2 instance.

## 8. Install Apache2

```bash
sudo apt install apache2
```
Installing Apache2 provides you with a powerful and widely used web server. Apache2 will be used to serve your 
Node.js application to the internet.

## 9. Change Ownership of /var/www
Override Apache's default permissions by changing ownership of /var/www:

```bash
sudo chown -R $USER:$USER /var/www
```
Changing the ownership of /var/www to your user ensures that you have the necessary permissions to deploy and manage 
files within Apache's web root directory.


## 10. Create Production Build
Create a production build of your Node.js application:

```bash
cd your-node-app
npm run build
```

Creating a production build compiles your Node.js application's source code into optimized and minified files. 
This ensures efficient and performant delivery to users' browsers.

## 11. Copy Build Folder to /var/www
Copy the build folder to the Apache web root directory:

```bash
sudo cp -r build /var/www
```
Copying the production build to /var/www makes your application's optimized files available to the Apache web 
server for serving to users.

## 12. Change Apache Configuration
Navigate to Apache's sites-available directory:

```bash
cd /etc/apache2/sites-available
```
Navigating to Apache's sites-available directory is necessary to access and edit Apache's configuration files.

## 13. Create New Site Configuration

```bash
sudo nano 001-reactjs-prod.conf
```

Inside the configuration file, add the following:

```bash
<VirtualHost *:80 >
ServerName """REPLACE HERE WITH YOUR PRODUCTION MACHINE PUBLIC IP"""
DocumentRoot /var/www/build
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Creating a new site configuration file allows you to define how Apache should handle requests for your Node.js application.
This configuration specifies the domain, document root, and other settings

## 14. Save the Configuration File
Save the configuration file and exit the text editor.


## 15. Activate the New Site

```bash
sudo a2ensite 001-reactjs-prod.conf
```
Activating the new site configuration enables Apache to use the settings defined in the
configuration file for serving your Node.js application.

## 16. Restart Apache
Restart the Apache server to apply the changes:

```bash
sudo systemctl restart apache2
```

Restarting the Apache web server is necessary to apply the configuration changes. After the restart, Apache will start serving your Node.js 
application using the new configuration.
