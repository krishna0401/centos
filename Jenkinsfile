# Jenkins 2.0 Buildfile
#
# Master Jenkins 2.0 can be started by typing:
# docker run -d -p 8090:8080 --name jenkins blacklabelops/jenkins
#
# Slave 'docker' can be started by typing:
# docker run -d -v /dev/vboxdrv:/dev/vboxdrv --link jenkins:jenkins -e "SWARM_CLIENT_LABELS=docker" blacklabelops/hashicorp-virtualbox
#
node('packer') {
  stage 'Clean'
  sh "./build/clean.sh"
  sh "./clean.sh"
  sh "vagrant up"

  stage 'Build'
  sh "rm -f blacklabelops-centos7.xz"
  sh "./build.sh"
  sh "./dockerbox/squashImage.sh"

  stage "Post-Clean"
  sh "vagrant destroy -f"
  sh "./clean.sh"
}
