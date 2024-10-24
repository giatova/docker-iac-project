# docker-iac-project
<img width="1075" alt="Screenshot 2024-10-24 at 1 15 11 PM" src="https://github.com/user-attachments/assets/10fbfd73-b94d-4cf4-ae8c-2b6178edf170">

![Screenshot 2024-10-24 at 1 13 07 PM](https://github.com/user-attachments/assets/ad03afde-0263-4137-91a4-a830816b9636)


![Screenshot 2024-10-24 at 1 13 33 PM](https://github.com/user-attachments/assets/0372cadc-b647-4553-b88a-dd0272710be7)

<img width="1512" alt="Screenshot 2024-10-24 at 1 19 03 PM" src="https://github.com/user-attachments/assets/7a05e9eb-80cd-41c1-af77-da554c79c4a4">


<img width="1512" alt="Screenshot 2024-10-24 at 1 19 11 PM" src="https://github.com/user-attachments/assets/129fd5ba-2435-461c-ac21-8702544a596a">


<img width="1512" alt="Screenshot 2024-10-24 at 1 18 27 PM" src="https://github.com/user-attachments/assets/6d1fa37d-6f30-4cc3-a7fb-6da1d6006a2a">


1) How would you get the Docker image from part 1 onto the AMI?
Build the Docker Image Make sure you have a Docker Image built on your local system. You can build the Docker Image: $ docker build -t my-image. Push the Docker Image to a registry Upload the Docker image to a container registry such as Docker Hub or Amazon Elastic Container Registry. docker tag my-image mydockerhubusername/my-image $ docker push mydockerhubusername/my-image. Install Docker on the EC2 instance Launch an EC2 instance using the AMI and install Docker if it is not already installed. $ sudo apt - get update $ sudo apt - get install docker.io. Pull the docker Container Image Once the Docker is installed on the EC2 instance, you can pull the Docker image from the registry: $ docker pull mydockerhubusername/my - image


2) How would you ensure the Docker container is started on boot, and all subsequent reboots of the EC2 instance?
1. Create a Docker Compose file or systemd service 2. Use systemd to Manage Docker systemctl create, for a 3. Enable the service to start on boot.


3) How would you keep the above AMI up to date with future package/security updates?
1. Package Manager: You can configure the EC2 instance itself to update packages automatically. We can do so by using “cron jobs”
• Run updates manually:
This will: sudo apt-get updatesudo apt-get upgrade -y
Or: Set a cron job to update it daily or weekly with the system.
sudo crontab -e
Insert this so that you can update the page daily
0 0 * * * apt-get update && apt-get upgrade -yPost Views: 2
2. Create AMIs on a Schedule: Automate the creation of new up to date AMI by
Automating AMI creation using either AWS Lambda or AWS Systems Manager.
• Alternatively, employ tools like Packer to automate the creation of new AMIs.


4) Assuming an EC2 instance exists with the above AMI running the Docker container
described in the first step, how would you go about ensuring the host EC2 instance is up
to date?
To make sure the EC2 instance has the latest software along with security patches:
1. Enable the yum-cron service: (For Amazon Linux or RedHat-based systems)
sudo yum install yum-cron
sudo systemctl enable yum-cron [Replace with appropriate name of the service]
sudo systemctl start yum-cron
2. Use Amazon CloudWatch and AWS Systems Manager to monitor instance health, and apply patches regularly.


5) Assuming an EC2 instance exists with the above AMI running the Docker container
described in the first step, how would you go about ensuring the Docker container is up
to date?
To confirm that de Docker container is up to date in our EC2 instance.
1. Container Registry: Pull the last latest version of image from container registry..
Download the imagedocker pull mydockerhubusername/my-image:latest
When you do not have to worry about finding newest version — then use tag :latest.
2. Docker Compose: Automatically Restart The Container With the Latest Image If you are using Docker compose, automatically docker will restart the container with latest image.
• Add these to your docker-compose. yml:
version: '3'
services:
  app:
Image: mydockerhubusername/my-image:latest
   restart: always
• Write a cron job to run this command and restart the container regularly.
3. Automation on Image Updates: With Docker Hub Webhooks, or AWS ECR Lifecycle Policies that automatically rebuild a new version of images and pull them down afterwards. 
