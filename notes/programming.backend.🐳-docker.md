---
id: riof1ujrx82zueb4tw4uulw
title: "\U0001F433 Docker"
desc: ''
updated: 1663409164559
created: 1663409164559
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: "\U0001F433 Docker"
updated_imported: '2022-03-29T07:11:22.000Z'
created_imported: '2019-11-24T09:21:19.000Z'
---

[TOC]


# Câu lệnh cơ bản của Docker

```bash
# Build 1 image
docker build --tag todo-app-backend:0.1 .


# run Image, if not have image, auto pull online
# -d meaning run in background
# -p port binding container-machine = 3000:80
docker run -d -p 80:80 nginx

# show all running docker containers
docker ps

# filter running docker
# grep used to search files for the occurrence of a string of characters that matches a specified pattern.
docker ps | grep nginx		

# remove running containers
# -f meaning force
docker rm -f ID_or_Name
docker rm -f 89898đf3981

# stop containers - Stop one or more running containers
docker stop [OPTIONS] CONTAINER [CONTAINER...]

#stop and remove all containers
docker rm -f $(docker ps -a -q)

# run and set name
# -d meaning run in background
docker run --name nginx -d -p 6969:6969 todo-app-backend:0.1	

# Run command inside running container in docker
# -it mean go inside container
docker exec -it todoapp sh
# or
docker exec -it a63cfbb59190 bash
/todoapp # ls -alh
/todoapp # node -v
# ctr + d to exit section

# list images
doker image ls -a 
# hoặc
docker images

# remove image
docker rmi Image-ID

# log error từ exited container 
docker logs 427a9d79scb9a

# Stop one or more running containers
docker stop myContainer [ my2ndcontainer...]


# ~ ~ ~ ~ ~ ~ ~ ~ ~ Clean up ~ ~ ~ ~ ~ ~ ~ ~ ~


# Clean up dangling Images
# Docker doesn’t have an automatic garbage collection system as of now.
# For now this command can be used to do a manual garbage collection.
docker rmi -f $(docker images -f "dangling=true" -q)
sudo docker rmi -f $(sudo docker images -f "dangling=true" -q)



# ~ ~ ~ ~ ~ ~ ~ ~ ~ Publishing a package ~ ~ ~ ~ ~ ~ ~ ~ ~ 

# List images to get image ID
docker images

> REPOSITORY           TAG      IMAGE ID      CREATED      SIZE
> monalisa             1.0      c75bebcdd211  4 weeks ago  1.11MB

# Tag the image id with OWNER/REPO/IMAGE_NAME
docker tag c75bebcdd211 docker.pkg.github.com/octocat/octo-app/monalisa:1.0

# Push the image to GitHub Packages
docker push docker.pkg.github.com/octocat/octo-app/monalisa:1.0

# Pull a package
docker pull docker.pkg.github.com/octocat/octo-app/monalisa:1.0


```


# Remove all images
All the Docker images on a system can be listed by adding `-a` to the docker images command.

Once you’re sure you want to delete them all, you can add the `-q` flag to pass the image ID to docker rmi:

```bash
# List:
docker images -a

# Remove:
docker rmi $(docker images -a -q)
```

[How To Remove Docker Images, Containers, and Volumes](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)

# Viết file cofiguration cho Image
Không làm manual được, ~> set 1 file configuration mô tả các step, gọi là Dockerfile ( ko extension ) trong thư mục source

```bash
# # 1 chọn HDH
# FROM ubuntu:18:04

# # 2 Cài đặt môi trường
# RUN apt install nodejs


# 1+2 or using directly the node Package
FROM node:12.16.1-alpine

# Thư mục làm việc
WORKDIR /todoapp/ 

# Copy code vào trong image
ADD package.json .		
# add file pacakage.json to workdir

# chạy code
# can run many as we want
RUN npm install --production

# Add source form current folder to the container
ADD src .

# Add debugger ( if meet any error )
RUN ls -alh


# Khởi chạy app
# can run just 1 time
CMD [ "npm", "start"]

# quyết định app chạy ở port nào, expose port đó
EXPOSE 6969
```


# How To Remove Docker Images, Containers, and Volumes

https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes

# Push images to github

## Tạo 1 github token

https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line

## Login to github
To keep your credentials secure, we recommend you save your personal access token in a local file on your computer and use Docker's --password-stdin flag, which reads your token from a local file.

```bash
cat ~/TOKEN.txt | docker login docker.pkg.github.com -u USERNAME --password-stdin
```
To use this example login command, replace USERNAME with your GitHub username and ~/TOKEN.txt with the file path to your personal access token for GitHub.

[source](https://help.github.com/en/packages/using-github-packages-with-your-projects-ecosystem/configuring-docker-for-use-with-github-packages)





# Run the Docker daemon as a non-root user (Rootless mode)
https://docs.docker.com/engine/security/rootless/

# series kiến thức về Docker

[https://viblo.asia/p/docker-chua-biet-gi-den-biet-dung-phan-1-lich-su-ByEZkWrEZQ0](https://viblo.asia/p/docker-chua-biet-gi-den-biet-dung-phan-1-lich-su-ByEZkWrEZQ0 "https://viblo.asia/p/docker-chua-biet-gi-den-biet-dung-phan-1-lich-su-ByEZkWrEZQ0")

# Error on rootless mode - Cannot acesss to localhost

Ports listening on localhost can be accessible from dockerd as 192.168.65.2 (VPNKit) or 10.0.2.2 (slirp4netns).

But you need to remove `--disable-host-loopback flag` for rootlesskit in `~/bin/dockerd-rootless.sh`:

[source](https://github.com/moby/moby/blob/1347481b9eb53c30f820df42a146aab8d06a7340/contrib/dockerd-rootless.sh#L85)

Also note that this allows containers to access all ports on the host localhost.

[source](https://github.com/docker/docker-install/issues/150) 


# Error gpg: no valid OpenPGP data found

Just for the record:

Same here: curl -fsSL [https://yum.dockerproject.org/gpg](https://yum.dockerproject.org/gpg "https://yum.dockerproject.org/gpg") | sudo apt-key add - gave

gpg: no valid OpenPGP data found.

Done it seprately with wget with success: wget [https://yum.dockerproject.org/gpg](https://yum.dockerproject.org/gpg "https://yum.dockerproject.org/gpg") 1.8k sudo apt-key add gpg

OK

[https://forums.docker.com/t/error-gpg-no-valid-openpgp-data-found/3857/2](https://forums.docker.com/t/error-gpg-no-valid-openpgp-data-found/3857/2 "https://forums.docker.com/t/error-gpg-no-valid-openpgp-data-found/3857/2")