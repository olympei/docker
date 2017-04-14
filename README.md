# docker
my docker tutorial

## Quick Reference


```
Remove dangling images
List :
docker images -f dangling=true
Remove :
docker rmi $(docker images -f dangling=true -q)


Remove all images :
docker rmi $(docker images -a -q)

Remove all stopped containers
docker rm $(docker ps -a -q)

Remove Issue :
[root@devel ~/GIT/docker]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              16.04               0ef2e08ed3fa        6 weeks ago         130 MB
ubuntu              latest              0ef2e08ed3fa        6 weeks ago         130 MB

[root@devel ~/GIT/docker]# docker rmi $(docker images -a -q)
Error response from daemon: conflict: unable to delete 0ef2e08ed3fa (must be forced) - image is referenced in multiple repositories
Error response from daemon: conflict: unable to delete 0ef2e08ed3fa (must be forced) - image is referenced in multiple repositories

Fixes :
[root@devel ~/GIT/docker]# docker rmi -f $(docker images -a -q)
Untagged: ubuntu:16.04
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:dd7808d8792c9841d0b460122f1acf0a2dd1f56404f8d1e56298048885e45535
Deleted: sha256:0ef2e08ed3fabfc44002ccb846c4f2416a2135affc3ce39538834059606f32dd
Deleted: sha256:0d58a35162057295d273c5fb8b7e26124a31588cdadad125f4bce63b638dddb5
Deleted: sha256:cb7f997e049c07cdd872b8354052c808499937645f6164912c4126015df036cc
Deleted: sha256:fcb4581c4f016b2e9761f8f69239433e1e123d6f5234ca9c30c33eba698487cc
Deleted: sha256:b53cd3273b78f7f9e7059231fe0a7ed52e0f8e3657363eb015c61b2a6942af87
Deleted: sha256:745f5be9952c1a22dd4225ed6c8d7b760fe0d3583efd52f91992463b53f7aea3

Docker Build Example :
root@devel:~/GIT/docker/mydockerbuild/ssh (master) $docker build -t debian-ssh .
root@devel:~/GIT/docker/mydockerbuild/ssh (master) $docker build -t debian-ssh:jessie .
root@devel:~/GIT/docker/mydockerbuild/ssh/debian/supervisor (master) $docker build -t debian/supervisor .


Keeping Docker running on host reboot
Apparently, the current method to auto-start Docker containers (from Docker 1.2) is to use restart policies. 
This will control how Docker should handle starting of the container upon startup and re-starting of the container when it exits. 
I've used the 'always' option so far, and can confirm that it makes Docker auto-start the container at system boot:

Syntax :
docker run --restart=always -d myimage

Example :
docker run --restart=always -d -p 50000:22 thakker/supervisor

Flag :
--restart=always
--restart=no (default)
--restart=on-failure
--restart=unless-stopped


###########################################################
# Disabling root access :
###########################################################

$docker run -d -p 50000:22 debian/ssh:dockerfile

root@devel:~ $docker ps
CONTAINER ID        IMAGE                   COMMAND               CREATED             STATUS              PORTS                   NAMES
a070065ccc9e        debian/ssh:dockerfile   "/usr/sbin/sshd -D"   6 seconds ago       Up 5 seconds        0.0.0.0:50000->22/tcp   dreamy_leakey


root@devel:~ $ssh root@172.17.0.1 -p 50000
root@172.17.0.1's password: 
-bash: warning: setlocale: LC_ALL: cannot change locale (en_US.UTF-8)
root@a070065ccc9e:~# 

Require apt-get install sudo first from root.


root@a070065ccc9e:~# adduser udemy
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = "en_US.UTF-8",
	LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
Adding user `udemy' ...
Adding new group `udemy' (1000) ...
Adding new user `udemy' (1000) with group `udemy' ...
Creating home directory `/home/udemy' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for udemy
Enter the new value, or press ENTER for the default
	Full Name []: Udemy
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] Y


root@a070065ccc9e:~# adduser udemy sudo
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = "en_US.UTF-8",
	LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
Adding user `udemy' to group `sudo' ...
Adding user udemy to group sudo
Done.

---------------------------------
To exit and save your changes 
---------------------------------

root@devel:~ $docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                   NAMES
42b334259ceb        debian/supervisor   "/usr/bin/supervis..."   4 minutes ago       Up 4 minutes        0.0.0.0:50000->22/tcp   quirky_euclid

root@devel:~ $docker commit quirky_euclid debian/supervisor
sha256:a14c15f7cece4c412a06fd41c7f0dbf4da26b4e9c34784996b1be5a0d7be5505
root@devel:~ $

###########################################################
# Easy consistent login setup:
###########################################################

root@devel:~ $vim ~/.ssh/config 
Host FirstContainer
Hostname 172.17.0.1
	User udemy
	port 50000
	StrictHostKeyChecking no
	
Test :
root@devel:~ $FirstContainer
-bash: FirstContainer: command not found
root@devel:~ $ssh FirstContainer
udemy@172.17.0.1's password: 
Last login: Fri Apr 14 18:15:41 2017
-bash: warning: setlocale: LC_ALL: cannot change locale (en_US.UTF-8)
udemy@42b334259ceb:~$ 


###########################################################
# Setup Nodejs with supervisor config:
###########################################################
~/.bashrc
alias nodehttpserver="docker run -d -p 50000:22 -p 59999:3000 -v /root/GIT/docker/mydockerbuild/ssh/debian/supervisor/nodejs/supervisor-config/:/etc/supervisor/conf.d/ debian/nodejs"

```



