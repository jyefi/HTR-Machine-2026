# HTR-Machine

 Docker image that isolates and automates the use of PyLaia and Kaldi toolkits for HTR.  

 ## IMPORTANT
This updated version of the original project contains a lot of serious issues. In other words, this Docker has been designed as an normal virtual machine, rather than being optimized to be used as a docker image.

## Requirements

- This software requires the installation of docker in the machine where it will be executed. Execute
the following comand in order to download and install the bleeding edge version of docker: 
	```
	curl -fsSL https://get.docker.com -o get-docker.sh && chmod +x get-docker.sh && sudo sh get-docker.sh
	```
	You can also install Docker following the specific instructions for your OS - [Docker CE](https://docs.docker.com/install/overview/)
- Optionally add your user to the docker group:
	```
	sudo usermod -aG docker $your_user
	```
	(adding a user to a group requires you to log out and log in order for the change to take effect)

--- 

## Use of the Docker Image

> Note that the use of sudo in the following commands is only required if the user has not been added to the docker group. 

1. In order to use the image (and launch a container)  you will need to first download (fastest) or build the container. 

* To download [jyefi/htr-machine-2026](https://hub.docker.com/r/jyefi/HTR-machine-2026) or pull the current version of the hub image simply execute the following command:
	```
	sudo docker pull jyefi/htr-machine-2026
	```
* To build the container clone this repository, go inside the downloaded repository and execute the following command: 
	```
	sudo docker build --rm  . -t htr-machine
	```
> Although downloading/building the docker image takes time it only needs to be performed once. If any changes are performed
in any of the files a recompilation of the image will be required.

2. Once the docker image is compiled/downloaded the basic way of using it is: 

* Using the image to login and perform complex experimentation (train, decode) 
	```
	sudo docker run --rm -it --name htr1 --mount 'type=bind,src=$workdir,dst=/work-dir' --gpus all --shm-size="16g" vbosch/htr-machine:latest
	```
* Using the image to automatically perform a task, this would require adding some sort of shellscript to build_resource directory, 
adding it to the docker especification (see line below), recompiling locally and running it as folows:
	```
    CMD . /root/.bashrc && conda init bash && conda activate pylaia && bash ./run.sh 

	sudo docker run --rm -it --name pala --mount 'type=bind,src=$workdir,dst=/work-dir' --gpus all --shm-size="16g" htr-machine:latest
	```
> To use the docker machine with outside data it is important to have all data required in a folder $foldir so it will be accessible inside the docker
container in the /work-dir folder. It is important to note that if you can only use docker via sudo you must chmod all generated files before leaving the 
session with:  
```
chmod -R 666 folder_name
```
If you forget to do this you can always log in into the docker machine again to change permissions. 

---

## Authors
* **Vicente Bosch ** - *Docker Image setup and testing*
* **Jaime ** 		 - *Upgrade image to 2026 libraries*
