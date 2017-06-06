# To demonstrate saving artifacts into gluster-s3 server.


## First bring up gluster-s3 docker container.

   Follow README in https://github.com/SaravanaStorageNetwork/docker-gluster-s3
   
   Make a note of IP address of the gluster-s3 service

---

## Build the build image container
git clone repo which have required changes

git clone https://github.com/SaravanaStorageNetwork/s2i-using-s3.git

\# cd  s2i-using-s3/examples/nginx-centos7/

Update the IP address of gluster-s3 service in s2i/bin/s3_access.

For example, updating address as 172.17.0.4: 

\# sed -i.bak '/^ip_address=/s/=.*/='\"172.17.0.4\"'/'  ./s2i/bin/s3_access

Now, build the builder container:

\# docker build -t nginx-centos7 .

## Verify incremental build by using run script:
\# IMAGE_NAME=nginx-centos7 test/run

---

## Changes in :

Dockerfile - for adding s3curl package as part of Docker container.

save-artifacts script - to upload a sample artifact to gluster s3 docker container. 

assemble - to restore artifact

test/run - to verify whether save and restore works. cleanup is commented out, so that we can login to docker and check whether
saved artifacts are present in /usr/share/nginx/html/

s3_access - to export gluster-s3 pod ip address.
