# Introduction to Containers and Docker v1.6

## Objectives
 - Use Cloud IAM to implement access control

 - Restrict access to specific features or resources

 - Use the Service Account User role

## Prerequisite
 - Sign in into Google Console with the credentials provided to you by Qwiklabs

### Tasks
 - SSH into the compute engine provided by Qwiklabs
 - From the console enter the following scripts

 `ls /`

 - This will log your VM directory is to verify the your Virtual Machine set up was completed
 - change directory to the source code provided by Qwiklabs

 `cd /kickstart` and `ls -lh` to view contents in the directory

 - Install the latest version of Python and PIP

 `sudo apt-get install -y python3 python3-pip`

 - Install Tornado library

 `pip3 install tornado`

 - Run the python application in the background
 
 `python3 web-server.py &`

 - Ensure the application is running by running the cURL command

 `curl http://localhost:8888`
 
 - Kill the application process 

 `kill %1`

 - Create a docker file

 `cat Dockerfile`

 - Build the docker image for the application

 `sudo docker build -t py-web-server:v1 .`

 - Run the application with docker 
 
 `sudo docker run -d -p 8888:8888 --name py-web-server -h my-web-server py-web-server:v1`

 - Access the application again

 `sudo docker run -d -p 8888:8888 --name py-web-server -h my-web-server py-web-server:v1`

 - Stop the application

 `sudo docker rm -f py-web-server`
 
 - Update the docker image to Google Cloud Registry

 `sudo usermod -aG docker $USER`

 - Exit the SSH session and create a new session

 - change directory into the source file provided

 `cd /kickstart`

 - Store the project name as an environment variable

 ``` export GCP_PROJECT=`gcloud config list core/project --format='value(core.project)'` ```

 - Build the docker image from Google Cloud Registry

 `docker build -t "gcr.io/${GCP_PROJECT}/py-web-server:v1" .`

 - Make the image publicly available

 `PATH=/usr/lib/google-cloud-sdk/bin:$PATH
gcloud auth configure-docker`

 - Push the image 

 `docker push gcr.io/${GCP_PROJECT}/py-web-server:v1`

 -  Verify the image has been stored to your storage by clicking on Storage from the Navigation menu

 - Update the permission to the Google Cloud Storage

 `gsutil defacl ch -u AllUsers:R gs://artifacts.${GCP_PROJECT}.appspot.com`

 `gsutil acl ch -r -u AllUsers:R gs://artifacts.${GCP_PROJECT}.appspot.com`

 `gsutil acl ch -u AllUsers:R gs://artifacts.${GCP_PROJECT}.appspot.com`

 - Run the application from any machine with the following scripts

 `docker run -d -p 8888:8888 -h my-web-server gcr.io/${GCP_PROJECT}/py-web-server:v1`