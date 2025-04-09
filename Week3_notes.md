# Week 3 key notes

## App deployment

- AWS Settings:
  - Application and OS image: Ubuntu 22.04
  - Security group:
    - Port 80 (HTTP)
    - Source: 0.0.0.0/0 (All possible IP addresses)
- Script:
```
#!/bin/bash

#update
sudo apt update -y

#upgrade add fix/mitigate
sudo NEEDRESTART_MODE=a apt-get dist-upgrade --yes

#install nginx
sudo apt install nginx -y

# $ and / characters must be escaped by putting a backslash before them
sudo sed -i "s/try_files \$uri \$uri\/ =404;/proxy_pass http:\/\/localhost:3000\/;/" /etc/nginx/sites-available/default

# restart nginx
sudo systemctl restart nginx

#enable nginx
sudo systemctl enable nginx

#get app code - cannot use scp (hint: use github and git)
git clone https://github.com/JamesC830/nodejs-sparta-test-app.git

#install nodejs v20.x 
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - &&\
sudo apt-get install -y nodejs

#check version
node -v

#cd into app folder
cd nodejs-sparta-test-app
cd app

#install app
sudo npm install

# install pm2 - process manager package for nodejs apps
sudo DEBIAN_FRONTEND=noninteractive npm install pm2 -g
 
# kill any running node processes that could interfere
pm2 kill
 
# run the app with pm2 (in the background by default)
pm2 start app.js

pm2 startup
```

Alternative purple screen fix:

```sudo sed -i 's/#$nrconf{restart} = '"'"'i'"'"';/$nrconf{restart} = '"'"'a'"'"';/g' /etc/needrestart/needrestart.conf```

or 

```Export DEBIAN_FRONTEND=noninteractive```

Changes the environment variable

## Database deployment

- AWS Settings:
  - Application and OS image: Ubuntu 22.04
  - Security group:
    - Port 27017 (mongoDB port)
    - Source: 0.0.0.0/0 (All possible IP addresses)
- Script:
```
#!/bin/bash

#update packages
sudo apt update -y

#upgrade packages
sudo NEEDRESTART_MODE=a apt-get dist-upgrade --yes

#install gnugp and curl
sudo NEEDRESTART_MODE=a apt install gnupg curl -y

#Add the gpg key
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor

#Create sources list file
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

#Update the package list again
sudo apt update -y

#install mongoDB
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y mongodb-org=7.0.6 mongodb-org-database=7.0.6 mongodb-org-server=7.0.6 mongodb-mongosh=2.1.5 mongodb-org-mongos=7.0.6 mongodb-org-tools=7.0.6

#Change the bindIP in the config file
sudo sed -i "s/bindIp: 127.0.0.1/bindIp: 0.0.0.0/" /etc/mongod.conf

#start mongoDB
sudo systemctl start mongod

#enable mongoDB
sudo systemctl enable mongod

#check its running
sudo systemctl status mongod
```

## User data

- A script which runs when an instance is created
- Need to make yourself the ubuntu user:
    - sudo /home/ubuntu
    - Alternative, see script
- Script:
```
#!/bin/bash

#Swap user to ubuntu
sudo -u ubuntu -i << EOF
echo "Running as the ubuntu user"

#Swap to ubuntu home directory
cd

#Wait for image script to run
sleep 20

cd nodejs-sparta-test-app
cd app

#Install npm
sudo npm install

sleep 2

#Need to manually insert <db-ip-address>
export DB_HOST=mongodb://<db-ip-address>:27017/posts
printenv DB_HOST

sleep 2

#Seed the posts database
node seeds/seed.js

sleep 2
 
# run the app with pm2 (in the background by default)
pm2 start app.js

#Stop being ubuntu as user
EOF
```

## Images/AMIs

- Blueprints for virtual machines (VMs), containing a complete operating system and pre-installed software
- Benefits:
  - Consistency
  - Faster deployment
  - Disaster recovery
  - Scalability
  - Cost efficiency
- Setting up an image:
  - The instance needs to be stopped when you create an image
  - The image will save all installed packages and dependancies
- Connecting to instance made from image:
  - Change root to ubuntu in connect command copies from AWS
  
## Auto-scaling

- ***Verical***: Bigger, more powerful CPU
- ***Horizontal***: More CPUs

![Image](../Cloud_comp_notes/Images/James_ASG.png)

- Launch template:
  - Gives the auto-scaling group all the information it needs about the VMs it needs to create (e.g. Image + User data + name resource tag)
- Auto-scaling group:
  - Creates virtual machines
  - Has scaling policy dictating how many VMs to make at any guven time
  - Will create VMs in different availablility zones to prevent the whole app going down
- Load balancer:
  - Distributes network traffic across multiple VMs to optimise performance

Setup order:
1. Launch template
2. Autoscaling group (load balancer is setup at the same time)

## VPCs

A private, isolated network within a public cloud infrastructure that allows an organization to isolate and control its own resources and network infrastructure.

Benefits:
- Security
- Ease of integration with on-prem infrastructure

Components:
- ***Subnets***: You can divide a VPC into subnets to organize and isolate resources based on various requirements
- ***Public vs Private Subnets***
  - ***Public***: Has direct access to the internet via internet gateway
  - ***Private***: Isolated from direct internet access. 
- CIDR blocks: Used to define the range of IP addresses available within a VPC or subnet
- ***Internet Gateways***: Allows communication between instances in your VPC and the internet
- ***Route Table/s***: A set of rules (called routes) used to determine where network traffic from your VPC is directed
- ***SG*** (Security group): Virtual firewalls used to control inbound and outbound traffic to AWS resources. Defines which ports are open to what traffic

Setup order:
1. VPC
2. Subnets
3. Internet gateway
4. Route tables
5. Instances

Delete order:
1. Terminate instances
2. Security groups (Found in the VPC dashboard)
3. VPC

Deleting the VPC will automatically delete:

1. Internet gateways
2. Route tables
3. Subnets