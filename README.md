Instructions for deploying the frontend and backend to somewhere publicly accessible over the internet  

Prerequsites
Gitbash terminal
AWS account

**Backend setup**
1. Using AWS, set up an EC2 instance using Ubuntu 18
2. Use the standard free-tier settings, create a new keypair and download
3. When you get to the security make sure you add a rule for Port 80 and 3000 so there will be four including the default Port 22 
4. Launch the EC2 instance
5. Once the instance is running, click Connect and copy the SSH client URL 
6. Put the key pair file in a folder on your personal laptop
7. Open the folder and right click in folder and click "Open with GitBash" 
8. While in the Git bash terminal, paste the SSH client URL from the AWS EC2 Connect page 
9. When prompted in the terminal type yes 
10. Clone the forked git repo into to your terminal by running the command: "git clone https://github.com/(username)/devops-code-challenge.git
11. Once the repo is cloned, run the command: "cd devops-code-challenge/" to move into cloned git repo for the set up
12. Run the command, "sudo apt-get update" to update the apt repo
13. Install nodejs and npm using the commands, respectively: "curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -" and then "sudo apt install -y nodejs"
14. Check if node.js and npm were successfully installed using the commands: node --version and npm --version 
15. Run the command sudo apt-get install nginx (nginx acts as a router)
16. Run the command sudo rm /etc/nginx/sites-enabled/default and sudo nano /etc/nginx/sites-available/(new name...i used "server") (creates new config file that I inserted the code 
17. server {
  listen 80;
  server_name tutorial;
  location / {
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header  Host       $http_host;
    proxy_pass        http://127.0.0.1:8080;
  }
} 
18. Run this command: sudo ln -s /etc/nginx/sites-available/server /etc/nginx/sites-enabled/server (this command links the new server config file in both directories sites-available and sites enable)
19. cd into backend folder and restart nginx
20. Restart nginx using the command: "sudo service nginx restart"
21. Using the EC2 instance's public DNS URL the server can be accessed on port 8080
22. Next is the PM2 configuration: Allows for Node.js process not to restart upon a crash or update
23. cd ../ out of the backend directory and install "npm i -g pm2"
24. Run the command "pm2 start index.js" in the backend directory to execute index.js 
25. In order to try starting it using "npm ci" and then "npm start", stop the server by using pm2 save
26. Run the commands "npm ci" and then "npm start" to start the backend component of the application
27. To access the project publicly I used the _instance public DNS URL_:8080

**Frontend setup we will be using AWS S3 and CLoudFront to deploy the frontend component **
1. Using steps 1-8 in the prior section open another Gitbash terminal from the same EC2 instance. From the starting screen cd into frontend directory of git repo. Run command "cd devops-code-challenge/frontend". Run commands "npm ci and npm start" Since npm was installed in the prior section we do not have to install again
3. Go to AWS S3 and create a new bucket (MAKE SURE TO TURN OFF BLOCK ALL PUBLIC ACCESS & ENABLE STATIC WEBSITE HOSTING)
4. Go to the forked git repo and upload all the files from the frontend directory to your new S3 bucket on AWS
5. Go to CloudFront and create new distribution connected to your S3 bucket
6. Using the static website URL in the S3 bucket under 'Properties' will show the deployed 







# Overview
This repository contains a React frontend, and an Express backend that the frontend connects to.

# Objective
Deploy the frontend and backend to somewhere publicly accessible over the internet. The AWS Free Tier should be more than sufficient to run this project, but you may use any platform and tooling you'd like for your solution.

Fork this repo as a base. You may change any code in this repository to suit the infrastructure you build in this code challenge.

# Submission
1. A github repo that has been forked from this repo with all your code.
2. Modify this README file with instructions for:
* Any tools needed to deploy your infrastructure
* All the steps needed to repeat your deployment process
* URLs to the your deployed frontend.

# Evaluation
You will be evaluated on the ease to replicate your infrastructure. This is a combination of quality of the instructions, as well as any scripts to automate the overall setup process.

# Setup your environment
Install nodejs. Binaries and installers can be found on nodejs.org.
https://nodejs.org/en/download/

For macOS or Linux, Nodejs can usually be found in your preferred package manager.
https://nodejs.org/en/download/package-manager/

Depending on the Linux distribution, the Node Package Manager `npm` may need to be installed separately.

# Running the project
The backend and the frontend will need to run on separate processes. The backend should be started first.
```
cd backend
npm ci
npm start
```
The backend should response to a GET request on `localhost:8080`.

With the backend started, the frontend can be started.
```
cd frontend
npm ci
npm start
```
The frontend can be accessed at `localhost:3000`. If the frontend successfully connects to the backend, a message saying "SUCCESS" followed by a guid should be displayed on the screen.  If the connection failed, an error message will be displayed on the screen.

# Configuration
The frontend has a configuration file at `frontend/src/config.js` that defines the URL to call the backend. This URL is used on `frontend/src/App.js#12`, where the front end will make the GET call during the initial load of the page.

The backend has a configuration file at `backend/config.js` that defines the host that the frontend will be calling from. This URL is used in the `Access-Control-Allow-Origin` CORS header, read in `backend/index.js#14`

# Optional Extras
The core requirement for this challenge is to get the provided application up and running for consumption over the public internet. That being said, there are some opportunities in this code challenge to demonstrate your skill sets that are above and beyond the core requirement.

A few examples of extras for this coding challenge:
1. Dockerizing the application
2. Scripts to set up the infrastructure
3. Providing a pipeline for the application deployment
4. Running the application in a serverless environment

This is not an exhaustive list of extra features that could be added to this code challenge. At the end of the day, this section is for you to demonstrate any skills you want to show that’s not captured in the core requirement.
