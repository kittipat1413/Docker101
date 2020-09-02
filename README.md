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
