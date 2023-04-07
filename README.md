# fmsdockerimg
Filemaker Server Docker Images


# Installation of FileMaker Server 19
Install FileMaker Server
Use the following steps to install FileMaker Server into the running container:

Step 1: Run the fmsdocker:prep image as a background, detached container.

The container will mount host directories as shared volumes. In the command below, replace <installation_file_dir> with your host's local directory that contains the Linux installation package for FileMaker Server. For example, if the Linux installation file is at /tmp/filemaker-server_19.4.2.204_amd64.deb on the host, replace <installation_file_dir> with /tmp. Similarly, if you will store data in the shared folder /Users/Tom/Database, replace <shared_FM_dir> with the actual shared folder name /Users/Tom/Database.

docker run                  \
    --detach                \
    --hostname   fms-docker \
    --name       fms-docker \
    --privileged            \
    --publish    80:80      \
    --publish    443:443    \
    --publish    2399:2399  \
    --publish    5003:5003  \
    --volume                \
        <installation_file_dir>:/install \
    --volume                \
        <shared_FM_dir>:"/opt/FileMaker/FileMaker Server/Data" \
    fmsdocker:prep
Step 2: Create a terminal session in the container with this command:

docker exec    \
    -it        \
    fms-docker \
    /bin/bash
You are now in a command shell within the fms-docker container, which was created from the fmsdocker:prep image.

Step 3: Install FileMaker Server.

Now that you have a container that has all the FileMaker Server dependencies, change the directory to the installation file's location and run the installation. This installation assumes you are using an assisted install with an "Assisted Install.txt" file. Prepare this file and put it in the <installation_file_dir> before executing the next commands. Replace <fms_package_file> with your installation file, which may look like the following file name: filemaker-server_19.4.2.204_amd64.deb.

cd /install
FM_ASSISTED_INSTALL=/install apt install /install/<fms_package_file>
After the installation, you can now exit the container shell.

Step 4: Create a new Docker image from the running container.

From your host command shell, create a new image from the running fms-docker container. This new image is the final image that contains all FileMaker Server dependencies. Use the following commands to create the new image:

docker commit fms-docker fmsdocker:final
docker stop fms-docker
docker container rm fms-docker