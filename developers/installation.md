---
description: Below you will find the installation steps on how to install Aleph.
---

# Installing Aleph

## Prerequisites

Aleph requires multiple services to operate. It uses Docker containers to make it easier for development and deployments. Before we continue, you will need to have Docker and `docker-compose` installed. Please refer to their manual to learn how to set up [Docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/).

## Developer setup

{% hint style="info" %}
This section describes how to set up Aleph for software development. Developer mode disables many security measures and it not meant to be used for internet-facing uses, [Production deployment](installation.md#production-deployment) is needed instead.
{% endhint %}

### What is developer mode?

Developer mode is a docker configuration for Aleph which makes it easy to do software development and debug the tool without having to install its dependencies on your host machine. These are the features of developer mode:

* The code for the backend \(api\) server and the React frontend will automatically reload to reflect any changes you make in your working copy while the application is running.
* Both backend and frontend will operate in debug mode and give more verbose error messages when a problem occurs.
* The host machine's file system will be accessible from within Aleph's docker container at `/host`.

Developer mode overrides the configuration file for `docker-compose`, using `docker-compose.dev.yml` instead of `docker-compose.yml`. This is done by wrapping most developer mode commands using `make`.

### Getting started

As a first step, check out the source code of Aleph from GitHub:

```bash
# Use the SSH URL if you have commit access:
git clone https://github.com/alephdata/aleph.git
cd aleph/
```

Once the code is downloaded, find the file called `aleph.env.tmpl` in the base directory. This is a template of the configuration file. Make a copy of this file named `aleph.env` and define settings for your local instance. Check [the configuration section](installation.md#configuration) for more information regarding the available options.

Also, please execute the following command to allow ElasticSearch to map its memory:

```bash
sudo sysctl -w vm.max_map_count=262144
```

With the settings in place, you can use `make all` to set everything up and launch the web service. This is equivalent to the following steps:

1. `make build` to build the docker images for the application and relevant services. You can run `make docker-pull` before to pull pre-build release images.
2. `make upgrade` to run the latest database migrations and create/update the search index.
3. `make web` to run the web-based API server and the user interface.

Open `http://localhost:8080/` in your browser to visit the web frontend.

If any of the above steps fail, refer to [the troubleshooting section](installation.md#troubleshooting) for some common stumbling blocks and their fixes.

### Working in a shell

During development, you will need to run command-line operations for certain tasks. In order to do so, you will first need to enter the docker container of Aleph. To do so, run:

```bash
make shell
# This will result in a root shell inside the container:
aleph --help
```

This will enter a docker container where the `aleph` shell command is available \(see [Usage](https://github.com/alephdata/aleph/wiki/Usage) for details\). You can also access the host computers file system at `/host`. This means a file stored at `/tmp/bla.txt` on your computer can be found at `/host/tmp/bla.txt` inside the container.

{% hint style="info" %}
When you run Aleph in development mode, the default configuration will not run the worker component used to index documents and do other background work. You can start it either via `make worker` or inside an Aleph shell using `aleph worker`.
{% endhint %}

### Users

For development purposes, you can quickly create a new user with the `aleph createuser` command, inside a shell:

```bash
aleph createuser --name="Alice" \
                 --admin \
                 --password=123abc \
                 user@example.com
```

If you pass an email address in the `ALEPH_ADMINS` environment variable \(in your [configuration](installation.md#system-configuration)\) it will automatically be made into an admin.

After running `createuser`, the newly created user's API key is printed, which you can use in the `Authorization` HTTP header of requests to the [API](). If you pass a password, you can use this email address and password to log into the web interface.

### Loading Sample Data

If you want to quickly get some sample data in your Aleph instance you can use `crawldir` to index a small test data folder.

```bash
aleph crawldir /aleph/contrib/testdata
```

To also get a sample of \(non-document\) entity data, consider loading [sanctions lists]().

### Running Tests

To run the tests, assuming you already have the `docker-compose` up and ready, run:

```bash
make test
make ingest-test
```

This will create a new database and run all the tests.

### Debugging

If you're looking to debug changes that you've made to Alephs python then there are a couple of options. By default, Aleph ships with the vscode python debugger (debugpy) enabled for the API and worker services when in dev mode. This makes it easy to create a launch.json file and attach a debugger to a running instance of the software.

The API is exposed via the standard 5678 port whereas the worker service is exposed via 5679.

Additionally you may wish to configure the debugger to wait for a client to attach. If this is the case you'll need to edit the docker-compose.dev.yml file adding the following as the command for the API:

```yaml
command: python3 -m debugpy --listen 0.0.0.0:5678 --wait-for-client -m flask run -h 0.0.0.0 -p 5000 --with-threads --reload --debugger
```

Or in the case of the worker, from the Makefile:

```bash
worker: services
	$(COMPOSE) run -p 127.0.0.1:5679:5679 --rm app python3 -m debugpy --listen 0.0.0.0:5679 --wait-for-client /usr/local/bin/aleph worker
```
If you'd prefer to use the pdb debugger then the first step is to add the following to the api section of the docker-compose.dev.yml:

```yaml
stdin_open: true
tty: true
```

Once this is done, restart your docker containers and set a breakpoint() in your code. Now, running docker attach aleph_api_1 should provide you the ability to view that breakpoint and make use of pdb's other features.
### Building from a clean state

You can also build the Aleph images locally. This could be useful while working on the Dockerfile changes and new dependency upgrades.

To build the image you can run `make build`, which will build the `alephdata/aleph` image \(this will generate a production ready image\).

## Production deployment

{% hint style="info" %}
This section details how to **set up Aleph in production mode**. If you plan to change the source code or quickly test the software, you may wish to use the [Developer setup](installation.md#developer-setup) instead.
{% endhint %}

Aleph is distributed as a set of Docker containers, which can be run on any server that meets the following criteria:

* 8GB \(or more\) of RAM. While the software will start with much less, we advise providing ample main memory for ideal performance.
* A working install of [Docker](https://www.docker.com/) and `docker-compose`. See [the FAQ page](technical-faq/#can-you-run-aleph-without-using-docker) for information on not using Docker.
* A domain name or IP address which can be used at the root via HTTPS \(i.e. Aleph doesn't support running at a sub-path like `/aleph`\). You are welcome to contribute fixes for this scenario.
* An internet connection to download and install the package.

To begin a production deployment:

* Obtain a copy of Aleph's [docker-compose](https://github.com/alephdata/aleph/blob/master/docker-compose.yml) file and [base configurations](https://github.com/alephdata/aleph/blob/master/aleph.env.tmpl) \(named `aleph.env.tmpl`\).
* Make a copy of the configurations file named `aleph.env` and define settings for your production instance. Check the [section on configuration](installation.md#configuration) for more information regarding the available options.

{% hint style="info" %}
Aleph has support for multiple storage engines, including Amazon Web Services Simple Storage Service, Google storage buckets, and local file storage. Remember to configure this in your instance's configuration file.
{% endhint %}

* Set `ALEPH_TAG` environment variable to specify the version of Aleph you want deploy. If `ALEPH_TAG` is not set, the stable version specified in the docker compose file is deployed.
* Once you are happy with your configuration, execute the following command to allow ElasticSearch to map its memory:

```bash
sudo sysctl -w vm.max_map_count=262144
```

* Finally, you can boot the docker-compose environment:

```bash
docker-compose up -d
```

This will run Aleph in detached mode. You can shut down the system at any time by issuing the following command:

```bash
docker-compose down
```

Before Aleph can process any requests or data, you need to make sure it sets up the database and search index correctly by executing an upgrade:

```bash
docker-compose run --rm shell aleph upgrade
```

While the system is running, it will bind to to port `8080` of its host machine and accept incoming connections. You can check that the system is functional with a curl request:

```bash
curl http://localhost:8080/api/2/statistics
```

If your servers firewall configuration allows it, you can now also open `http://localhost:8080` in your browser and use the web interface to navigate the application.

{% hint style="warning" %}
Be careful with the ports exposed from your Docker system on public ports. Docker is known to override some firewall rules so make sure you double-check that you're only exposing the intended ports on your productions system.
{% endhint %}

### Upgrading

See the [relevant section in the Technical FAQ](technical-faq/#how-can-i-upgrade-to-a-new-version-of-aleph).

## Configuration

The main configuration file of Aleph is `aleph.env`, which is loaded by docker-compose and can modify many aspects of system behaviour. A template for the configuration with details regarding many of the options is available in the `aleph.env.tmpl` file.

### Configuration options

* You will need to provide a value for the `ALEPH_SECRET_KEY`. A good example of a value is the output of `openssl rand -hex 24`.
* Aleph needs to know the URL under which the web interface is mounted. Make sure to set the correct public `ALEPH_UI_URL`.

### OAuth Credentials

Using OAuth for login is optional. Skip this section \(and leave the config commented out\) if you don't want to use it.

Aleph supports a couple of OAuth providers out of the box: Google, Facebook and Microsoft Azure.

#### Using Google OAuth

To get the OAuth credentials please visit the [Google Developers Console](https://console.developers.google.com/). There you will need to [create an API key](https://support.google.com/googleapi/answer/6158862). In the **Authorised redirect URIs** section, use this URL:

```text
http://localhost:8080/api/2/sessions/callback/google
```

Save the client ID and the client secret as `ALEPH_OAUTH_*` values.

#### Using Microsoft Azure

Create a new app over at [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/), make a note of the KEY and secret you generate there. Callback URL should be as follows: https:///api/2/sessions/callback . Then add the following to aleph.env, remember to update KEY and SECRET with your values.

```text
ALEPH_OAUTH=true
ALEPH_OAUTH_KEY=***************************
ALEPH_OAUTH_SECRET=***************************
ALEPH_OAUTH_NAME=Azure
ALEPH_OAUTH_BASE_URL=https://login.microsoftonline.com/organizations/oauth2/v2.0/
ALEPH_OAUTH_AUTHORIZE_URL=https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize
ALEPH_OAUTH_TOKEN_URL=https://login.microsoftonline.com/common/oauth2/token
ALEPH_OAUTH_SCOPE=openid profile email user.read
ALEPH_OAUTH_TOKEN_METHOD=POST
```

## Troubleshooting

Troubleshooting help can be found in the [Technical FAQ](technical-faq/).

