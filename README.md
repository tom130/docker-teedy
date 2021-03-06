# Teedy multi-architecture Docker images

Teedy (formerly Sismics Docs) is an open source, lightweight document management system. The source code and project can be found here - [https://github.com/sismics/docs](https://github.com/sismics/docs).

Prebuilt images are available on Docker Hub as [jdreinhardt/teedy](https://hub.docker.com/r/jdreinhardt/teedy)

Default credentials are `admin:admin` Make sure to update the default password after login.

## README

The `build.sh` is designed to simplify the generation of multi-architecture Docker images of Teedy. It also includes a number of safety checks and automations to simplify the build process. A few customizations are available as variables at the top of the `build.sh` script.

Currently supported architectures are 
 - `amd64 (x86_64)`
 - `arm32v7 (armhf)`
 - `arm64 (aarch64)`

This list is not exhaustive and more can easily be added at the top of the script, or built manually. 

The `Dockerfile` uses a builder image to reduce the overall size of the image while also building Teedy directly from source ensure the images stay up-to-date with the source project. 

### Build Pattern

The `build.sh` script is the easiest way to build images yourself. For manual builds you will need to enable buildx on your system. Once you enable buildx you will need to also run the following:
 - `docker run --rm --privileged docker/binfmt:a7996909642ee92942dcd6cff44b9b95f08dad64` (This is only required if you want to build for architectures other than your build system)
 - `docker buildx create --name xbuilder`
 - `docker buildx use xbuilder`
 - `docker buildx inspect --bootstrap`

After running these commands you will have a new buildx builder running. You can use `docker buildx ls` to view all builders and their supported architectures.

No build arguments are required. An example build command would be `docker buildx build -f Dockerfile --platform=linux/arm/v7,linux/amd64,linux/arm64 --push -t jdreinhardt/teedy:latest .`

### Usage Pattern

The following parameters are available 
 - `-e JAVA_OPTIONS` customize the Java Options (default: -Xmx512m)
 - `-p 8080` web interface internal port
 - `-v /data` Teedy data location

Example run command `docker run -e JAVA_OPTIONS=-Xmx1024m -p 80:8080 -v /mnt/teedy:/data jdreinhardt/teedy:latest`
