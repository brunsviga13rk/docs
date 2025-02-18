
# Build & Run

This chapter explains the process of building and deployment of the emulator
software. First, let's discuss the core architecture of the software.
The emulator is a web application written in Typescript that uses various
frameworks to achieve state managements, 3d rendering, icons and widgets.
React is the core component that manages states and is responsible for client
side rendering. Widgets are based on MUI and 3d rendering is achieved through
three.js which utilizes WebGL under the hood.

!!! info

    The emulator relies soley on client side rendering.
    For this reason only a file server is required for deployments.

## Building locally

Vite is used as the build system and packages are managed by `npm`. Before
building the application make sure to install all required dependencies which
are specified in the `package.json` file. In order to perform a clean installation
of the dependencies run:

```shell
npm ci
```

This will create a folder `node_modules` and download all dependencies into it.
Afterward we can start building the application by running:

```
npm run build
```

The build process will compile the Typescript source into Javascript perform
optimizations and simplify the code. The resulting artifacts will be stored
in the `dist` folder. This folder contains all source code and static assets
needed to run the application. In order to so start a web server in the `dist`
folder and load the `index.html` page.

Alternatively you can use `vite` development build to build and run directly
by running:

```shell
npm run dev
```

This will start a web server on port `5173`. This mode is specifically useful
for testing as it will live reload the application when the source files
change.

## OCI container

The repository also contains a `Dockerfile` which is used for each release to
build an OCI compatible container image containing the prebuild application
and static assets. This image is uploaded to the GitHub container registry.
The latest version of the emulator image can be pulled this way:

```shell
docker pull ghcr.io/brunsviga13rk/emulator:latest
```

!!! info

    The Dockerfile and container image works with both Docker and Podman.
    `docker` can, in all commands in this chapter, be interchanged with `podman`.

You can also build the image yourself locally through docker:

```
docker build --tag brunsviga13rk/emulator:git .
```

The docker image used Nginx as static file server and listens by default on
port 80. In order to run the container and access the application make sure
to bind the containers port 80 to a host port:

```shell
docker run -p "8080:80" ghcr.io/brunsviga13rk/emulator:latest
```

Now you can access the application under `http://localhost:8080`

## Deployment

The application can be deployed through any web server capable of serving
static files such as: Apache, Nginx, Caddy, ...
This allows to deploy the application through GitHub pages which can only
server static sites. The demo application running the latest version is
available [here](https://brunsviga13rk.github.io/emulator).
