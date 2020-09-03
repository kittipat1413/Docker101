# Docker101

## Docker RUN vs CMD vs ENTRYPOINT
   Some Docker instructions look similar and cause confusion among developers who just started using Docker or do it irregularly. In this post I will explain the difference                             between CMD, RUN, and ENTRYPOINT on examples.

## In a nutshell
   RUN executes command(s) in a new layer and creates a new image. E.g., it is often used for installing software packages.
   CMD sets default command and/or parameters, which can be overwritten from command line when docker container runs.
   ENTRYPOINT configures a container that will run as an executable.
   If it doesn’t make much sense or you after details, then read on.
    
## Docker images and layers
   When Docker runs a container, it runs an image inside it. This image is usually built by executing Docker instructions, which add layers on top of existing image or OS distribution. OS distribution is the initial image and every added layer creates a new image.
   Final Docker image reminds an onion with OS distribution inside and a number of layers on top of it. For example, your image can be built by installing a number of deb packages and your application on top of Ubuntu 14.04 distribution.

## Shell and Exec forms
   All three instructions (RUN, CMD and ENTRYPOINT) can be specified in shell form or exec form. Let’s get familiar with these forms first, because the forms usually cause more confusion than instructions themselves.
## Shell form

**`<instruction> <command>`**

### Examples:
```bash
RUN apt-get install python3
CMD echo "Hello world"
ENTRYPOINT echo "Hello world"
```
When instruction is executed in shell form it calls /bin/sh -c <command> under the hood and normal shell processing happens. For example, the following snippet in Dockerfile
```bash
ENV name John Dow
ENTRYPOINT echo "Hello, $name"
```
when container runs as **`docker run -it <image>`** will produce output
```bash
Hello, John Dow
```
Note that variable name is replaced with its value.

## Exec form

This is the preferred form for CMD and ENTRYPOINT instructions.

**`<instruction> ["executable", "param1", "param2", ...]`**

### Examples:
```bash
RUN ["apt-get", "install", "python3"]
CMD ["/bin/echo", "Hello world"]
ENTRYPOINT ["/bin/echo", "Hello world"]
```
When instruction is executed in exec form it calls executable directly, and shell processing does not happen. For example, the following snippet in Dockerfile
```bash
ENV name John Dow
ENTRYPOINT ["/bin/echo", "Hello, $name"]
```
when container runs as **`docker run -it <image>`** will produce output
```bash
Hello, $name
```
Note that variable name is not substituted.

### How to run bash?

If you need to run bash (or any other interpreter but sh), use exec form with **`/bin/bash`** as executable. In this case, normal shell processing will take place. For example, the following snippet in Dockerfile
```bash
ENV name John Dow
ENTRYPOINT ["/bin/bash", "-c", "echo Hello, $name"]
```
when container runs as **`docker run -it <image>`** will produce output
```bash
Hello, John Dow
```
