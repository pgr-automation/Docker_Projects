Here's a guide to all commonly used Dockerfile instructions with explanations of what each one does:
Basic Instructions

    FROM
        Syntax: FROM <image>[:<tag>]
        Purpose: Defines the base image for the Docker image being built. The tag specifies the version (default is latest).
        Example: FROM ubuntu:20.04

    LABEL
        Syntax: LABEL <key>=<value>
        Purpose: Adds metadata to the image, useful for storing information like author or project details.
        Example: LABEL maintainer="you@example.com"

    ARG
        Syntax: ARG <name>[=<default>]
        Purpose: Defines variables that users can pass at build-time to the Dockerfile using the --build-arg flag.
        Example:

    ARG NODE_VERSION=14
    RUN apt-get install -y nodejs=$NODE_VERSION

ENV

    Syntax: ENV <key>=<value>
    Purpose: Sets environment variables in the image, making them accessible in all subsequent RUN instructions and to containers at runtime.
    Example: ENV PATH="/app/bin:${PATH}"

COPY

    Syntax: COPY <source> <destination>
    Purpose: Copies files or directories from the local file system into the container.
    Example: COPY . /app

ADD

    Syntax: ADD <source> <destination>
    Purpose: Similar to COPY, but also allows for fetching remote URLs and auto-extracting tar archives.
    Example: ADD https://example.com/file.tar.gz /app/

RUN

    Syntax: RUN <command>
    Purpose: Executes commands in a new layer on top of the current image and commits the results.
    Example: RUN apt-get update && apt-get install -y python3

CMD

    Syntax: CMD ["executable", "param1", "param2"]
    Purpose: Provides the default command to run when the container starts. Only one CMD is allowed, and it can be overridden by docker run.
    Example: CMD ["python3", "app.py"]

ENTRYPOINT

    Syntax: ENTRYPOINT ["executable", "param1", "param2"]
    Purpose: Defines the command that will always run when the container starts, even if arguments are passed in docker run. It’s typically used in conjunction with CMD.
    Example:

        ENTRYPOINT ["python3"]
        CMD ["app.py"]

    WORKDIR
        Syntax: WORKDIR <path>
        Purpose: Sets the working directory inside the container for all following instructions.
        Example: WORKDIR /app

Advanced Instructions

    EXPOSE
        Syntax: EXPOSE <port>
        Purpose: Informs Docker that the container listens on the specified network ports at runtime. This does not publish the port.
        Example: EXPOSE 80

    VOLUME
        Syntax: VOLUME ["/data"]
        Purpose: Creates a mount point with a specified path and marks it as holding externally stored data.
        Example: VOLUME /data

    USER
        Syntax: USER <username>[:<group>]
        Purpose: Sets the user and group for running following instructions or when the container starts.
        Example: USER appuser

    ONBUILD
        Syntax: ONBUILD <instruction>
        Purpose: Adds a trigger instruction to the image. When an image with an ONBUILD instruction is used as a base image, the specified instruction is executed.
        Example: ONBUILD RUN echo "Image built"

    STOPSIGNAL
        Syntax: STOPSIGNAL <signal>
        Purpose: Specifies the system call signal to stop the container.
        Example: STOPSIGNAL SIGTERM

    HEALTHCHECK
        Syntax: HEALTHCHECK [options] CMD <command>
        Purpose: Checks if the container is healthy and still operational.
        Example:

    HEALTHCHECK --interval=30s --timeout=10s \
    CMD curl -f http://localhost/ || exit 1

SHELL

    Syntax: SHELL ["executable", "parameters"]
    Purpose: Allows you to specify a different shell for the following RUN commands.
    Example: SHELL ["powershell", "-command"]