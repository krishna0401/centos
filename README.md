
# Docker CentOS 7 Base Image

[![Docker Repository on Quay.io](https://quay.io/repository/blacklabelops/centos/status "Docker Repository on Quay")](https://quay.io/repository/blacklabelops/centos) [![Docker Stars](https://img.shields.io/docker/stars/blacklabelops/centos.svg)](https://hub.docker.com/r/blacklabelops/centos/) [![Docker Pulls](https://img.shields.io/docker/pulls/blacklabelops/centos.svg)](https://hub.docker.com/r/blacklabelops/centos/)

## Release: blacklabelops/centos:latest

In my view the most flexible way to build Docker Base Images. This project builds docker base images from kickstart files.

I have wrapped this [CentOS Tutorial](https://github.com/CentOS/sig-cloud-instance-build/tree/master/docker) inside a working Vagrant box to build my own centos base images. It uses [KatzJ Ami-Creator](https://github.com/katzj/ami-creator) to build images and extracts them with [Guestfish](http://libguestfs.org/guestfish.1.html) inside Docker compatible tar balls.

Builds the Docker Image [blacklabelops/centos](https://atlas.hashicorp.com/blacklabelops/boxes/dockerdev) Image on Dockerhub.

## Features

* Ready-to-Use Vagrant box with working software.
* Full control over the build with AMI kickstart files.
   * Free choice of distribution
	* Control over Repositories
	* Control over Packages, Timezone and Locals
	* CentOS: Remove and Disable SystemD

## Usage

Spin up the box and log in!

~~~~
$ vagrant up
$ vagrant ssh
~~~~    

Change into the mounted project folder.

~~~~
$ cd /vagrant
~~~~

Build the Image!

~~~~
$ ./buildBaseImage.sh blacklabelops-centos7.ks blacklabelops-centos7
~~~~   

Extract the image in a tarball.

~~~~
$ ./extractImage.sh blacklabelops-centos7.img blacklabelops-centos7
~~~~

Now its time to exit the box!

~~~~
$ exit
~~~~

Importing the tarball into docker:

~~~~
$ cat blacklabelops-centos7.xz | docker import - blacklabelops/centos
~~~~

Optional: Update and Squash the CentOS base image!

~~~~
$ ./dockerbox/squashImage.sh
~~~~

## Update Security Conflicted Packages

The base image does not contain the latest CentOS packages! The kickstart file and the amicreator are not able to update packages during image generation!

This introduces new problems because new dependent images are not able to update base packages. The update routine will result in an error saying that dependend systemd-container packages are conflicting.

In order to retrieve an archive of latest packages I have written the following update script:

~~~~
$ ./dockerbox/squashImage.sh
~~~~

> This will build a docker image and update all packages, the result will be squashed inside a new xz-archive.

## References

* [Imagelayers.io](https://imagelayers.io/?images=blacklabelops%2Fcentos:latest)
* [Blacklabelops Docker CenOS Image](https://registry.hub.docker.com/u/blacklabelops/centos/)
* [KatzJ Ami-Creator](https://github.com/katzj/ami-creator)
* [Guestfish](http://libguestfs.org/guestfish.1.html)
* [Vagrant Homepage](https://www.vagrantup.com/)
* [Virtualbox Homepage](https://www.virtualbox.org/)
* [Docker Homepage](https://www.docker.com/)
