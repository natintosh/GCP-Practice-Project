# Cloud IAM

## Objectives
 - Use Cloud IAM to implement access control

 - Restrict access to specific features or resources

 - Use the Service Account User role

## Prerequisite
 - Sign in into Google Console with the credentials provided to you by Qwiklabs

### Task 1: Setup for two users
 - Sign into a second account with the credentials provided by Qwiklabs

### Task 2: Explore the IAM console
 - Goto IAM from IAM & admin from the Navigation menu from the Google Cloud Platform
 - Click on Add button and note the roles associated with each roles
 - Switch to the second account
 - Goto IAM from IAM & admin and compare the associated roles names with Account 1 and Account 2
 - Switch back to the first account

### Task 3: Prepare a resource for access testing
 - Create a bucket from Storage > Browser
 - Upload a sample file from your Storage from your machine
 - Rename the file to `sample.txt`  
 - Switch to Account 2 and verify that the account can view the file


 ### Task 4: Remove project access
 - Switch to Account 1
 - Remove Account 2 it associated roles 
 - Switch back to Account 2 and verify that the account does not have access to the file

### Task 5: Add storage access
 - Switch to the first account
 - Add Account 2 as a new member from the IAM page which can be accessed from IAM & admin from the Navigation menu
 - Select Cloud Storage > Storage Object Viewer for Account 2 and save
 - Verify that Account 2 has access to the file by using the Cloud Shell with the script below

 `gsutil ls gs://[YOUR_BUCKET_NAME]`

 n.b. [YOUR_BUCKET_NAME] was the name you provided while creating a bucket from Task 3.

### Task 6: Set up the Service Account User
 - Switch back to Account 1
 - Create a new service account and name it as read-bucket-objects.
 - specific the role for the service account as `Cloud Storage` then `Storage Object Viewer`
 - Select the newly create service account 
 - Add a new member in the permission panel with it role as `Service Accounts > Service Account User`
 - Grant the service account Compute Engine Access
 - Create a new Virtual Machine instance from `Compute Engine > VM instances` on the Navigation menu

### Task 7: Explore the Service Account User role
 - SSH to the newly created Virtual Machine
 - Run the following script

 `gcloud compute instances list`
 
 - You will see an error about some request not completing the reason is because the new Service account does not access to list compute engine instances

 - Run the following script to copy the file from your bucket

 `gsutil cp gs://[YOUR_BUCKET_NAME]/sample.txt .`

 - The file `sample.txt` would be copies to the root of your VM instances

 - Rename the file with the following script

 `mv sample.txt sample2.txt`

 - Run the following script to save the new file to your bucket

 `gsutil cp sample2.txt gs://[YOUR_BUCKET_NAME]`

 - you will get a error because the service account does not have access to write into the bucket but only read access
  
