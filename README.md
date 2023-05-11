# docker_kodeKloud

## Basic commands


```cmd
docker run "image"
```

- Process Status
    - ps: this will show running images process status
    - ps -a: ths will show runnng and previously executed images process status
```cmd
docker ps
docker ps -a
```

- stop runnig containers
- remove stoped and exited  container permanently
```cmd
docker stop "container_id"
docker rm "container_id"
```

- list of available images
- remove an image
    - for remove an image we have to stop and remove all dependent containers first
```cmd
docker images
docker rmi 'image'
```

- run in detached mode (run in background)
```cmd
docker run -d 'image'
```


