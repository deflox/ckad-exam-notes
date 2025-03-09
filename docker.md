```
# pull image:
docker image pull <image>:<tag>

# save image to other format instead of OCI format
docker save <image>:<tag> --output <name>.tar

# save state of a running container to an image
docker commit <container> <image-name>

# rename tag used to name an image
docker image tag <old> <new>

# delete image (only possible if no running container uses this image)
docker image rm <image-name>
```