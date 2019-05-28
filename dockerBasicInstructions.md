Example with a docker MAF image (LSST)

Install Docker
Download and install docker if needed

https://docs.docker.com/install/

Move to the directory where you want your work to be saved and copy MAF docker image prepared by Owen Boberg which contains MAF and OpSim installations as well as jupyter notebooks !
 

```
docker pull fedhere/dockertest
```

NB. Note that the docker images are stored in the root partition rather than in the directory where you ran the docker command. If you want to change that (i.e. if you are low on space on the root partition), edit /etc/default/docker (or wherever your OS keeps the daemon directives) and add DOCKER_OPTS=”-g /path/to/images”. Then restart the docker service with sudo service docker restart.

Start the docker 

docker run -it --name dod_container \
	                -v ${PWD}:/home/docname/mydoc_local \
	                -p 8888:8888 \
	                 fedhere/dockertest

- *docker run -it --name my_container_name* : tells docker to start a container in an interactive mode (-it) with the name (--name) my_container_name. You can change the name to whatever you would like.


- *-v ${PWD}:/home/docname/mydoc_local* : will mount the present working directory PWD inside the container as /home/docmaf/maf_local. While in the container only things saved in /home/docmaf/maf_local will be saved to your local machine when the container is stopped.


- *-p 8888:8888* : maps port 8888 inside the container to port 8888 on your local machine. This will allow you to run jupyter notebooks in your local browser.


- *fedhere/dockertest:latest* tells docker which image to use when running the container.


I should see the docker prompt now: something like [docname@6f6717863b52 ~]
Check where I am : 

[docname@6f6717863b52 ~]$ pwd
	/home/docname

I’m inside the docker so I can setup

Move to the directory with the installation

[docmaf@6f6717863b52 ~]$ cd mydoc_local/	
[docmaf@6f6717863b52 maf_local]$ pwd
/home/docname/mydoc_local


