# docker-jenkins-sonar-images
Image to use as a test environment for using Jenkins, Sonar and Git with several platforms

## Contents

This repo contains a lot of files, the files below are the most important:

| File         | Description                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dockerfile   | File used by Docker to (re)create the image.                                                                                                                                                                                                                                                                                                                                                                                           |
| jenkins_home | Directory with Jenkins configuration. This is a folder with a large size, it is used as a volume (see step 3a/3b) to preserve jobs and configuration.
 - Pre-configured jobs in jobs-folder - Pre-configured SonarQube Runner and SonarQube server connection (config.xml) - Plugins pre-installed in plugins-folder - .ssh folder with keys to authenticate to use the git-repository from the pre-configured jobs - credentials.xml that points the jobs to the right SSH-key to use - plugins.txt containing the names and versions of plugins to use |
| root         | Folder containing fix-permission script to make the jenkins user owner                                                                                                                                                                                                                                                                                                                                                                 |
| jenkins.sh   | Bash script to start Jenkins to run. When attached to Docker container you can run this script with & from any location to use it as a background process.                                                                                                                                                                                                                                                                             |
| run.sh       | Bash script to start SonarQube to run. When attached to Docker container you can run this script with & from the location /opt/sonarqube/bin to use it as a background process.                                                                                                                                                                                                                                                        |

## Prerequisites

* Docker
* ssh-keygen (part of OpenSSH)

## Steps to use this image
1. Clone this repo
1a. Type ```docker pull rodmidde/docker-jenkins-sonar-images:rodmidde_jenkins-sonar-image-cplusplus``` to pull/download a pre-built image.
1b. Alternative: Type ```docker build .``` to locally build the image, the docker process returns a image id (e.g. b4b0100b2905).

2. Create a SSH RSA keypair. Register your public key in BitBucket in your [account](https://git.icaprojecten.nl/stash/plugins/servlet/ssh/account/keys). Rename your private key to id_rsa_rojenkins and overwrite this file at jenkins_home/.ssh/id_rsa_rojenkins

3a. Started with 1a?

3b. Started with 1b? Start docker in the root of this repository with ```docker run -v ABSOLUTE_PATH_TO_THE_CLONED_REPO/jenkins_home:/var/jenkins_home -p 9000:9000 -p 8080:8080 -it b4b0100b2905 /bin/bash```
