# Install Docker
 
1. sudo apt-get install linux-image-extra-$(name -r)
1. sudo apt-get install linux-image-extra-virtual
1. sudo apt-get update
1. sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
1. curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
1. sudo apt-key fingerprint 0EBFCD88
1. sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
1. sudo apt-get update
1. sudo apt-get install docker-ce
1. docker build -t cryptomove .
1. docker run -ti --privileged cryptomove /bin/bash

# Mount an S3 bucket to an EC2 instance

Instructions for mounting a block device in AWS: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html

1. wget https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/s3fs/s3fs-1.74.tar.gz
1. tar -xvzf s3fs-*.tar.gz
1. sudo apt-get update
1. sudo apt-get install build-essential gcc libfuse-dev libcurl4-openssl-dev libxml2-dev mime-support pkg-config libxml++2.6-dev libssl-dev
1. cd s3fs-*
1. ./configure --prefix=/usr
1. make
1. make install
 
# Check the directory for which the s3fs was mounted:
 
1. which s3fs
 
# Configuring S3 bucket with access key and secret key
 
Create a new file in /etc with the name passwd-s3fs and paste the access key & secret key in the below format.
 
1. touch /etc/passwd-s3fs
1. vim /etc/passwd-s3fs
1. Your_accesskey:Your_secretkey
 
# Change the permissions of the file
 
1. sudo chmod 640 /etc/passwd-s3fs
 
# Create a directory and mount S3bucket in it. Provide your S3 bucket name in place of <your_bucketname>
 
1. mkdir /mys3bucket
1. s3fs <your_bucketname> -o use_cache=/tmp -o allow_other -o multireq_max=5 /mys3bucket
 
# Change CryptoMove datastore to AWS S3
 
1. export HELLO_PACKSRC_PATH=/home/Ubuntu/cmove
1. mv /home/Ubuntu/cmove/datstore /mnt/mys3bucket/datastore
1. export CRYPTOMOVE_PATH=/mnt/mys3bucket/datastore
