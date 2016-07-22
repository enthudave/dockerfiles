# WORK IN PROGRESS

# dockerfiles

#### The images with *eclim* have to be run with `/sbin/my_init -- su - dev` as the command

For example:

1. Inside a folder containing the Dockerfile and the ./service/xfvb folder,
    `docker build -t <IMAGE-NAME> .`

2. Now you can run it like,
    `docker run -it <IMAGE-NAME> /sbin/my_init -- su - dev`
