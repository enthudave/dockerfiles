# dockerfiles

These images have to be run with ' /sbin/my_init -- su - dev' as the command

For example:

 Inside a folder containing the Dockerfile and the ./service/xfvb folder,
  docker build -t <IMAGE-NAME> .

 Now you can run it like,
  docker run -it <IMAGE-NAME> /sbin/my_init -- su - dev
