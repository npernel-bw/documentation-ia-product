---
layout: page
title: "Debian"
parent: "Installation"
grand_parent: "Packaging"
toc: true
---

Before installing the solution it is necessary to configure the timezone of your instance:  

```sh  
sudo timedatectl set-timezone Europe/Paris
```

# Install Docker Runtime

To install the docker runtime you can execute these commands:

```sh
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo gpasswd -a $(whoami) docker
```

> :warning: Log out and log back in, to make sure your user gets the permissions to run docker commands

# Creating Users are Required Directories

It is recommended to create a set of system users and groups to install the brainwave CLI.

```sh
sudo addgroup --system brainwave
sudo adduser --system --ingroup brainwave --no-create-home --disabled-password brainwave
```

Create the following directories:

```sh
sudo mkdir -p /var/log/brainwave
sudo mkdir -p /var/lib/brainwave
sudo mkdir -p /etc/brainwave
sudo mkdir -p /usr/local/brainwave
```

Set the owner of the new directories:

```sh
sudo chown -R brainwave:brainwave /var/log/brainwave
sudo chown -R brainwave:brainwave /var/lib/brainwave
sudo chown -R brainwave:brainwave /etc/brainwave
sudo chown -R brainwave:brainwave /usr/local/brainwave
```

Set the permissions

```sh
sudo chmod ug+rwx -R /var/log/brainwave
sudo chmod ug+rwx -R /var/lib/brainwave
sudo chmod ug+rwx -R /etc/brainwave
sudo chmod ug+rwx -R /usr/local/brainwave
```

# Download and Install Brainwave CLI

Download the Brainwave tools binary and its corresponding sha256 file to verify the download, from Brainwave's Gitea repository [https://repository.brainwavegrc.com/](https://repository.brainwavegrc.com/). You can also use the following direct link:  

[https://repository.brainwavegrc.com/Brainwave/-/packages/generic/brainwavetools_linux_amd64/1.2](https://repository.brainwavegrc.com/Brainwave/-/packages/generic/brainwavetools_linux_amd64/1.2)

Verify the download

```sh
echo "$(cat brainwave.sha256)  brainwave" | sha256sum --check
```

Installation

```sh
sudo mkdir -p /usr/local/bin
sudo cp brainwave /usr/local/bin/brainwave
sudo chown brainwave:brainwave /usr/local/bin/brainwave
sudo chmod ug+rx /usr/local/bin/brainwave
```

Add the current user to the brainwave group

```sh
sudo gpasswd -a $(whoami) brainwave
```

> :warning: Log out and log back in, to make sure your user gets the permissions to run brainwave commands

# Brainwave Registry

It is necessary to log into the docker registry for Brainwave to be able to pull the desired images:  

```sh
docker login repository.brainwavegrc.com
```

# Installation

To install the solution in server mode please use the following command:  

```sh
brainwave install --project-name brainwave --server
brainwave pull
```

# Initial configuration

> <span style="color:grey">**Note:**</span> The hostname `brainwave.local` used below is provided as an example for the command line. The parameter should be updated to your context, the hostname of the machine hosting the docker service.  
> The hostname value **must** be `lowercase`  

Before starting the services, set up the hostname

```sh
brainwave config --hostname brainwave.local
```

# TLS configuration (Optional)

To activate tls  

```sh
brainwave config --tls
```

Before starting the service it is necessary to copy the certificate and the private key into the certificates folder

```sh
cp brainwave.local.key /etc/brainwave/certificates/brainwave.local.key
cp brainwave.local.crt /etc/brainwave/certificates/brainwave.local.crt
```

> Note that the filename musth match the domain name. More generally the files have to be named `<hostname>.crt` and `<hostname>.key`

Please refer to the following page for more information:

[SSL configuration page](igrc-platform/installation-and-deployment/packaging/configuration/ssl-configuration.md){: .ref}

# Starting the Services

```sh
brainwave start
```

Once installed navigate to the `/config` webpage to finalize the configuration. Please see [here](igrc-platform/installation-and-deployment/packaging/configuration/config-ui.md) for more information.  
