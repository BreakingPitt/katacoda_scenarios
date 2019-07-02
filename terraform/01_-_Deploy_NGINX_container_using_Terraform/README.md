
# Deploy NGINX container using Terraform

## Create Terraform Config

Terraform works based on a configuration file, in this case config.tf.
The configuration defines your infrastructure, in this instance as providers
and resources.

A provider is an abstract way of handling the underlying infrastructure and
responsible for managing the lifecycle of a resource.

A resource are components of your infrastructure, for example a container or
image.

The editor allows us to write the config.tf file. The first section defines a
docker host where we want to apply our configuration too.

```
provider "docker" {
  host = "tcp://docker:2345/"
}
```

We can now start defining the resources of our infrastructure.
The first resource is our Docker image. A resource has two parameters, one
is a TYPE and second a NAME. The type is docker_image and the name is nginx.

Within the block we define the name and tag of the Docker Image.

```
resource "docker_image" "nginx" {
  name = "nginx:1.11-alpine"
}
```

We can define our container resource. The resource type is docker_container
and name as nginx-server. Within the block we set the resource parameters.

We can reference other resources, such as a the image.

```
resource "docker_container" "nginx-server" {
  name = "nginx-server"
  image = "${docker_image.nginx.latest}"
  ports {
    internal = 80
  }
  volumes {
    container_path  = "/usr/share/nginx/html"
    host_path = "/home/scrapbook/tutorial/www"
    read_only = true
  }
}
```

## Plan Terraform Actions

Once the configuration has been defined we need to create an execution plan.
Terraform describes the actions required to achieve the desired state.

The plan can be saved using -out. We'll apply the execution plan in the next
step.

To create a plan, use the CLI

```
terraform plan -out config.tfplan
```

```
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but
will not be persisted to local or remote state storage.


The Terraform execution plan has been generated and is shown below.
Resources are shown in alphabetical order for quick scanning. Green resources
will be created (or destroyed and then created if an existing resource
exists), yellow resources are being changed in-place, and red resources
will be destroyed. Cyan entries are data sources to be read.

Your plan was also saved to the path below. Call the "apply" subcommand
with this plan file and Terraform will exactly execute this execution
plan.

Path: config.tfplan

+ docker_container.nginx_server.0
    bridge:                            "<computed>"
    gateway:                           "<computed>"
    image:                             "${docker_image.nginx.latest}"
    ip_address:                        "<computed>"
    ip_prefix_length:                  "<computed>"
    log_driver:                        "json-file"
    must_run:                          "true"
    name:                              "nginx-server-1"
    ports.#:                           "1"
    ports.985670353.external:          ""
    ports.985670353.internal:          "80"
    ports.985670353.ip:                ""
    ports.985670353.protocol:          "tcp"
    restart:                           "no"
    volumes.#:                         "1"
    volumes.3224431186.container_path: "/usr/share/nginx/html"
    volumes.3224431186.from_container: ""
    volumes.3224431186.host_path:      "/home/scrapbook/tutorial/www"
    volumes.3224431186.read_only:      "true"
    volumes.3224431186.volume_name:    ""

+ docker_container.nginx_server.1
    bridge:                            "<computed>"
    gateway:                           "<computed>"
    image:                             "${docker_image.nginx.latest}"
    ip_address:                        "<computed>"
    ip_prefix_length:                  "<computed>"
    log_driver:                        "json-file"
    must_run:                          "true"
    name:                              "nginx-server-2"
    ports.#:                           "1"
    ports.985670353.external:          ""
    ports.985670353.internal:          "80"
    ports.985670353.ip:                ""
    ports.985670353.protocol:          "tcp"
    restart:                           "no"
    volumes.#:                         "1"
    volumes.3224431186.container_path: "/usr/share/nginx/html"
    volumes.3224431186.from_container: ""
    volumes.3224431186.host_path:      "/home/scrapbook/tutorial/www"
    volumes.3224431186.read_only:      "true"
    volumes.3224431186.volume_name:    ""

+ docker_image.nginx
    latest: "<computed>"
    name:   "nginx:1.11-alpine"


Plan: 3 to add, 0 to change, 0 to destroy.
```

The output of the command indicates the changes. In this case, you'll see a
_+ dockercontainer.nginx-server and _+ dockerimage.nginx to highlight adding
the new resources.

Finally a summary of Plan: 2 to add, 0 to change, 0 to destroy.

## Apply Terraform Actions

Once the plan has been created we need to apply it to reach our desired state.

Using the CLI, terraform will pull any images required and launch new
containers.

```
terraform apply
```

The output will indicate the changes and the resulting configuration.

```
docker_image.nginx: Creating...
  latest: "" => "<computed>"
  name:   "" => "nginx:1.11-alpine"
docker_image.nginx: Still creating... (10s elapsed)
docker_image.nginx: Creation complete
docker_container.nginx_server.1: Creating...
  bridge:                            "" => "<computed>"
  gateway:                           "" => "<computed>"
  image:                             "" => "sha256:bedece1f06cc142829698e6ba8f04d7f92e7f1b94b985e14b65f55e6ae4858c2"
  ip_address:                        "" => "<computed>"
  ip_prefix_length:                  "" => "<computed>"
  log_driver:                        "" => "json-file"
  must_run:                          "" => "true"
  name:                              "" => "nginx-server-2"
  ports.#:                           "" => "1"
  ports.985670353.external:          "" => ""
  ports.985670353.internal:          "" => "80"
  ports.985670353.ip:                "" => ""
  ports.985670353.protocol:          "" => "tcp"
  restart:                           "" => "no"
  volumes.#:                         "" => "1"
  volumes.3224431186.container_path: "" => "/usr/share/nginx/html"
  volumes.3224431186.from_container: "" => ""
  volumes.3224431186.host_path:      "" => "/home/scrapbook/tutorial/www"
  volumes.3224431186.read_only:      "" => "true"
  volumes.3224431186.volume_name:    "" => ""
docker_container.nginx_server.0: Creating...
  bridge:                            "" => "<computed>"
  gateway:                           "" => "<computed>"
  image:                             "" => "sha256:bedece1f06cc142829698e6ba8f04d7f92e7f1b94b985e14b65f55e6ae4858c2"
  ip_address:                        "" => "<computed>"
  ip_prefix_length:                  "" => "<computed>"
  log_driver:                        "" => "json-file"
  must_run:                          "" => "true"
  name:                              "" => "nginx-server-1"
  ports.#:                           "" => "1"
  ports.985670353.external:          "" => ""
  ports.985670353.internal:          "" => "80"
  ports.985670353.ip:                "" => ""
  ports.985670353.protocol:          "" => "tcp"
  restart:                           "" => "no"
  volumes.#:                         "" => "1"
  volumes.3224431186.container_path: "" => "/usr/share/nginx/html"
  volumes.3224431186.from_container: "" => ""
  volumes.3224431186.host_path:      "" => "/home/scrapbook/tutorial/www"
  volumes.3224431186.read_only:      "" => "true"
  volumes.3224431186.volume_name:    "" => ""
docker_container.nginx_server.0: Creation complete
docker_container.nginx_server.1: Creation complete

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

The state of your infrastructure has been saved to the path
below. This state is required to modify and destroy your
infrastructure, so keep it safe. To inspect the complete state
use the `terraform show` command.

State path: terraform.tfstate
```

## Inspecting Infrastructure
You can use the Docker CLI to view the changes and see the newly launched
container.

```
docker ps
```

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
77900be7fc14        bedece1f06cc        "nginx -g 'daemon ..."   2 minutes ago       Up 2 minutes        443/tcp, 0.0.0.0:32768->80/tcp   nginx-server
```

You can inspect this in future using the terraform CLI

```
terraform show
```

```
docker_container.nginx_server.0:
  id = 77900be7fc14dc560009082c53f4948ab8902b718f00d86a450cdc294c7c7790
  bridge =
  gateway = 172.18.0.1
  image = sha256:bedece1f06cc142829698e6ba8f04d7f92e7f1b94b985e14b65f55e6ae4858c2
  ip_address = 172.18.0.2
  ip_prefix_length = 24
  log_driver = json-file
  must_run = true
  name = nginx-server-1
  ports.# = 1
  ports.985670353.external = 0
  ports.985670353.internal = 80
  ports.985670353.ip =
  ports.985670353.protocol = tcp
  restart = no
  volumes.# = 1
  volumes.3224431186.container_path = /usr/share/nginx/html
  volumes.3224431186.from_container =
  volumes.3224431186.host_path = /home/scrapbook/tutorial/www
  volumes.3224431186.read_only = true
  volumes.3224431186.volume_name =
docker_container.nginx_server.1:
  id = aca0c9d1329156460b590b6124a8a7afe4cc202be6c130f5e3b313ba94df8533
  bridge =
  gateway = 172.18.0.1
  image = sha256:bedece1f06cc142829698e6ba8f04d7f92e7f1b94b985e14b65f55e6ae4858c2
  ip_address = 172.18.0.3
  ip_prefix_length = 24
  log_driver = json-file
  must_run = true
  name = nginx-server-2
  ports.# = 1
  ports.985670353.external = 0
  ports.985670353.internal = 80
  ports.985670353.ip =
  ports.985670353.protocol = tcp
  restart = no
  volumes.# = 1
  volumes.3224431186.container_path = /usr/share/nginx/html
  volumes.3224431186.from_container =
  volumes.3224431186.host_path = /home/scrapbook/tutorial/www
  volumes.3224431186.read_only = true
  volumes.3224431186.volume_name =
docker_image.nginx:
  id = sha256:bedece1f06cc142829698e6ba8f04d7f92e7f1b94b985e14b65f55e6ae4858c2nginx:1.11-alpine
  latest = sha256:bedece1f06cc142829698e6ba8f04d7f92e7f1b94b985e14b65f55e6ae4858c2
  name = nginx:1.11-alpine
```

## Updating Infrastructure

As our infrastructure grows and changes, terraform will manage and ensure
we always have our defined desired state.

We can change our container to launch two instances, each with different names.

```
resource "docker_container" "nginx-server" {
  count = 2
  name = "nginx-server-${count.index+1}"
  image = "${docker_image.nginx.latest}"
  ports {
    internal = 80
  }
  volumes {
    container_path  = "/usr/share/nginx/html"
    host_path = "/home/scrapbook/tutorial/www"
    read_only = true
  }
}
```

If we create a plan you will see the actions Terraform will need to apply
to adapt our infrastructure to match our configuration.


```
terraform plan -out config.tfplan
```

The plan will outline the changes. Because we're changing the name and adding a
resource we'll see Plan: 1 to add, 1 to change, 0 to destroy.

In the details it will explain that changing a container name forces the
resource to be recreated name: "nginx-server" => "nginx-server-1"
(forces new resource) along with adding the new container
_+ dockercontainer.nginx-server.1

We can then apply the plan as we did in the previous step.


```
docker ps
```

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
77900be7fc14        bedece1f06cc        "nginx -g 'daemon ..."   9 minutes ago       Up 9 minutes        443/tcp, 0.0.0.0:32768->80/tcp   nginx-server-1
aca0c9d13291        bedece1f06cc        "nginx -g 'daemon ..."   9 minutes ago       Up 9 minutes        443/tcp, 0.0.0.0:32769->80/tcp   nginx-server-2
```
