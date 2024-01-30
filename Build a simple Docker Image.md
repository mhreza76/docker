
## Build a simple Docker Image

**What is a Docker Image?**\
A Docker image is a read-only template that contains a set of instructions for creating a container that can run on the Docker platform.

**What is docker base image?**\
A Docker base image is the initial image used as the foundation for building a Docker container.\
It serves as the starting point from which your application and its dependencies are added to create a runnable environment.\
Base images are typically pre-configured operating system images with certain tools, libraries, and settings already installed.

**Here's a basic Dockerfile for a Python "Hello, World!" application.**

**Steps 1** Create a `app.py` file and add print("Hello, World!")

`echo 'print("Hello, World!")' > app.py`

**Steps 2** Create a `Dockerfile` and Dockerize Python `Hello, World!` application.
```bash
# Use an official Python runtime as a parent image
FROM python:3.8-slim
# Set the working directory in the container
WORKDIR /app
# Copy the current directory contents into the container at /app
COPY . /app
# Run app.py when the container launches
CMD ["python", "app.py"]
```
**Steps 3** Build and run docker image.
`docker build -t rezabaiust/hello-python:001`
`docker run rezabaiust/hello-python:001`