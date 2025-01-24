# Q1 Run docker with the python:3.12.8 image in an interactive mode, use the entrypoint bash. What's the version of pip in the image?

$ docker run -it --entrypoint=bash python:3.12.8 \
$ pip --version \
pip 24.3.1 

# Q2 Given the following docker-compose.yaml, what is the hostname and port that pgadmin should use to connect to the postgres database?

db:5432
