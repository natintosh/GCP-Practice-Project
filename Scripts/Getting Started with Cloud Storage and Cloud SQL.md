# Getting Started with Cloud Storage and Cloud SQL

## Objectives
 - Use Cloud IAM to implement access control

 - Restrict access to specific features or resources

 - Use the Service Account User role

## Prerequisite and Task 1
 - Sign in into Google Console with the credentials provided to you by Qwiklabs


### Task 2: Deploy a web server VM instance
 - Create a new Virtual Machine and enter the script below for as the start up script

 ```bash
    apt-get update
    apt-get install apache2 php php-mysql -y
    service apache2 restart
 ```

### Task 3: Create a Cloud Storage bucket using the gsutil command line
 - Open the Cloud Shell from the Google Cloud Platform and export your location as an environment variable

 ```bash
    export LOCATION=US
 ```

  - Create a new bucket with the following script

  ```bash 
    gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
  ```
  - Copy a banner image to your Cloud Shell memory
  
  ```bash
    gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
  ```
  
  - Copy the image to your bucket storage 

  ```bash
    gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
  ```

  - Set the access to global readable to all users

  ```bash
    gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
  ```

 ### Task 4: Create the Cloud SQL instance
  - Create a new SQL instance with id and password `blog-db`, password of your choice respectively

  - Set the region to the same region the Virtual Machine is located

  - Copy the public IP address of the SQL instance

  - Create a new user `blogdbuser` and type a password of your choice 

  - Click on the connections tab and add a new network 

  - Set the name to `web front end`

  - For the network, set it to the external IP address of your `bloghost` VM instance, followed by /32 and save

### Task 5: Configure an application in a Compute Engine instance to use Cloud SQL

 - SSH into the VM instance
 - Change your directory to

 ```bash
    cd /var/www/html
 ```

 - Edit `index.php` with nano editor

 ```bash
    sudo nano index.php
 ```

 - Paste the following content into the editor and save

 ```php
<html>
<head><title>Welcome to my excellent blog</title></head>
<body>
<h1>Welcome to my excellent blog</h1>
<?php
 $dbserver = "CLOUDSQLIP";
$dbuser = "blogdbuser";
$dbpassword = "DBPASSWORD";
// In a production blog, we would not store the MySQL
// password in the document root. Instead, we would store it in a
// configuration file elsewhere on the web server VM instance.

$conn = new mysqli($dbserver, $dbuser, $dbpassword);

if (mysqli_connect_error()) {
        echo ("Database connection failed: " . mysqli_connect_error());
} else {
        echo ("Database connection succeeded.");
}
?>
</body></html>
 ```

 - Restart the web server

 ```bash
    sudo service apache2 restart
 ```

 - Open a new tab and access the content with the external IP address with `/index.php`

 - You will get a error message on the webpage and this is because you have not add your SQL instance name, user and password

 - Change your directory to

 ```bash
    cd /var/www/html
 ```

 - Edit `index.php` with nano editor

 ```bash
    sudo nano index.php
 ```
 - Update `dbserver`, `dbuser`, `dbpassword` to their respective values


 - Restart the web server

 ```bash
    sudo service apache2 restart
 ```

 - Reload the webpage from the other tab and you will find a success message

### Task 6: Configure an application in a Compute Engine instance to use a Cloud Storage object

 - Goto your bucket storage
 - Copy the public link for the image you uploaded
 
 - SSH into the VM instance
 - Change your directory to

 ```bash
    cd /var/www/html
 ```

 - Edit `index.php` with nano editor

 ```bash
    sudo nano index.php
 ```
 - Paste the following code above the `h1` tag

 ```php
    <img src='[public link to image]'>
 ```

 - Save an exit the editor

 - Restart the web server

 ```bash
    sudo service apache2 restart
 ```

 - Reload the webpage from the other tab and you will find an image