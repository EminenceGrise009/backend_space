# backend_space

Amazon S3 Integration:
Create an S3 Bucket:

Go to the AWS Management Console.
Navigate to the S3 service.
Click "Create bucket."
Provide a unique bucket name, choose your desired region, and configure other settings as needed.
Configure AWS Credentials:

Create an IAM user with permissions to access S3.
Note down the Access Key ID and Secret Access Key for the IAM user.
Configure Environment Variables:

Store the Access Key ID, Secret Access Key, and S3 bucket name as environment variables in your Node.js application.


export AWS_ACCESS_KEY_ID=your_access_key_id
export AWS_SECRET_ACCESS_KEY=your_secret_access_key
export S3_BUCKET_NAME=your_bucket_name



EC2 Deployment:
Launch an EC2 Instance:

Go to the AWS Management Console.
Navigate to the EC2 service.
Click "Launch Instance" and select an Amazon Machine Image (AMI) for your instance.
Configure instance details (instance type, VPC, subnet, etc.).
Configure security groups to allow incoming traffic to the required ports (e.g., port 80 for HTTP).
Create or select an existing key pair for SSH access.
Connect to Your EC2 Instance:

Use SSH to connect to your EC2 instance using the key pair you configured.
The command will look like: ssh -i "your-key.pem" ec2-user@your-instance-ip.
Set Up Your Environment:

Update the system and install necessary packages:

sudo yum update -y
sudo yum install git -y
Clone your project repository onto the EC2 instance:

git clone your-repo-url
cd your-project-directory
Install Node.js and npm using NVM:

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
source ~/.bashrc
nvm install node
Install PM2 for Process Management:

Install PM2 to keep your Node.js application running:

npm install -g pm2
Run Your Application with PM2:

Start your application using PM2:

pm2 start app.js
Save the process list to ensure your application restarts after a system reboot:

pm2 save
Configure Nginx as Reverse Proxy:

Install Nginx to act as a reverse proxy:

sudo yum install nginx -y
Create an Nginx configuration file in /etc/nginx/conf.d/your-app.conf:

sudo nano /etc/nginx/conf.d/your-app.conf
Add the following configuration:
nginx

server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
Restart Nginx:

sudo systemctl restart nginx
Set Up Domain (Optional):

If you have a domain, configure your domain's DNS settings to point to your EC2 instance's IP address.
Secure Your EC2 Instance:

Configure a firewall (Security Group) to allow only necessary incoming traffic.
Set up HTTPS using SSL certificates (you can use services like Let's Encrypt).
Environment Variables:
To handle environment variables in your Node.js application, you can use a package like dotenv. Here's how to set it up:

Install dotenv:


npm install dotenv
Create a .env file in your project directory and add your environment variables:


AWS_ACCESS_KEY_ID=your_access_key_id
AWS_SECRET_ACCESS_KEY=your_secret_access_key
S3_BUCKET_NAME=your_bucket_name
In your Node.js code (e.g., app.js), load the environment variables using dotenv:

require('dotenv').config();
Now you can access the environment variables using process.env.AWS_ACCESS_KEY_ID, process.env.AWS_SECRET_ACCESS_KEY, and process.env.S3_BUCKET_NAME in your code.
