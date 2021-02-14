# Hexo demo

[Hexo.io](https://hexo.io/) static site generator demo with Docker. Read my blog 
[Troubleshooting Docker](https://pen-y-fan.github.io/2021/02/12/Troubleshooting-Docker/) for more details.

## Requirements

- Node.js 12 using [Docker](https://docs.docker.com/get-docker/)
- [Hexo](https://hexo.io/) has been installed as a project dependency

## Installation

Clone the repository

```sh
git clone git@github.com:Pen-y-Fan/hexo-demo.git
cd hexo-demo
echo $(id -u)
# check your user id is 1000, if different edit Makefile and change UID=1000
make shell-run
# inside the Docker container /app install the dependencies
yarn 
```

## Quick Start

### Run server

The server can be started on port 4000

```shell script
# with Make and Docker
make server
```

Once started open the browser <localhost:4000>

More info: [Server](https://hexo.io/docs/server.html)

### Docker information

`make server` Will start the container equivalent to:

`docker run -d -p 4000:4000 -u $(id -u):$(id -g) -v $(pwd):/app -w /app --rm node:12.18.4-alpine yarn server`

- `-d` starts the docker container in a demon (background) 
- `-p 4000:4000` on port 4000 and expose port 4000
- `-u $(id -u):$(id -g)` under the current user's account
- `-v $(pwd):/app` map the current directory to /app in the container
- `-w /app` make /app the working directory
- `--rm` when the container is closed it will be removed
- `node:12.18.4-alpine` the node 12 LTS release using the lightweight Alpine OS
- `yarn server` runs the package.json script `hexo server` 

Note: Node 14 LTS has a [Regression in Node 14](https://github.com/hexojs/hexo/issues/4257), therefore the
**composer-compose.yml** has been configured to use Node 12 until this is resolved.

### Create a new post

Options, depending on node being installed, with or without but hexo, or docker can be used, via make command.

```shell script
# or with Make and Docker:
make hexo new=My-New-Post
```

**Note**: The `title` will need to be manually updated in the My-New-Post.md file

More info: [Writing](https://hexo.io/docs/writing.html)

### Generate static files

Options, depending on node being installed, with or without but hexo, or docker can be used, via make command

```shell script
# with Make/Docker
make generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

## Deploy

See [github pages](https://pages.github.com/) on how to deploy a project.

## Stopping the server

Once finished run

```shell
make down
```

This will run `docker-compose down --remove-orphans`
