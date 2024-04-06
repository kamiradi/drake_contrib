# Drake contribution test environment
---

I use this environment to keep track of various docker images for testing
`drake`, `manipulation` and `underactuated`. Usually this requires quickly
mounting a fresh branch from that deviates from the master, therefore I dont
clone the repositories. I manage individual repositories through submodules.

### Setting up the repo
To setup the repository, clone it into your local folder and then inside the
parent folder of the repo run the following
```
git submodule update --init --recursive
```

## Workflow
---
### Common Docker commands
The following are some of the standard docker commands that are useful for
developing using docker
The following command attaches to absh shell within a container that goes by
`CONTAINER_NAME`
```
docker attach CONTAINER_NAME
```

### Drake
To just load up the drake container first build it with the following
```
docker-compose -f .docker/drake.nightly.yaml build
```
Next, run the container using
```
docker-compose -f .docker/drake.nightly.yaml up
```
Open a shell into the container either using portainer or one of the above
docker commands. I use portainer if I am only running a script/binary from
within a container. For build jobs it is better to attach to a container's
terminal.
Next, Set up the container for the build
```
./setup/ubuntu/install_prereqs.sh
```
This should prompt you for a bunch of permissions, reply by typing `Y/n`. In
case you want to go ahead and build the repository, the following commands are
useful
```
cd path/to/drake_workspace
bazel build //... # build the entire project
bazel test //... # build and test the entire project

# just build relevant binaries and run the planar scenegrpah test
bazel test -j 2 //bindings/pydrake/systems:py/planar_scenegraph_visualizer_test
# or
bazel test -j HOST_CPUS*0.5 //bindings/pydrake/systems:py/planar_scenegraph_visualizer_test
```


### Manipulation
Run the docker container according to the following command (build it if you
haven't already)
```
docker-compose -f .docker/manipulation.nightly.yaml up
```

shell into the running container and activate pyenv with the following command
```
eval "$(/pyenv/bin/pyenv init -)"
```
Once this is done activate the manipulation environment
```
# from the manipulation folder
source .venv/bin/activate
```

