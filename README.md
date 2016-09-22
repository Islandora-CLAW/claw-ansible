# Islandora CLAW: Ansible

[![Contribution Guidelines](http://img.shields.io/badge/CONTRIBUTING-Guidelines-blue.svg)](./CONTRIBUTING.md)
[![LICENSE](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](./LICENSE)

# :heavy_exclamation_mark: :heavy_exclamation_mark: :heavy_exclamation_mark:

Since Islandora CLAW has moved development to Drupal 8, our Ansible build **no longer** functions properly. We recommend using the [vagrant](https://github.com/Islandora-CLAW/CLAW/tree/master/install) build instead. Please follow the Islandora [listserv](https://groups.google.com/forum/?hl=en&fromgroups=#!forum/islandora) and weekly [CLAW Tech Calls](https://github.com/Islandora-CLAW/CLAW/wiki#islandora-claw-tech-calls) for updates about the status of Docker and Ansible with Islandora CLAW. 

 Please note that even with vagrant, there is not yet a stable release of Islandora CLAW. We welcome your volunteer contributions to help get the project to production. 
 
 :heavy_exclamation_mark: :heavy_exclamation_mark: :heavy_exclamation_mark:
 
## Introduction

The is a development environment for the Islandora and Fedora 4 project (CLAW).

Vagrant is used for launching the environment, and we currently support three providers:

1. Virtualbox
2. Digital Ocean
3. AWS

For each of these providers only Ubuntu 14.04 is currently supported, though plans for supporting CentOS 7+ as well as Docker as an additional provider are in the works.

*N.B.* This virtual machine **should not** be used in production **yet**.

## Includes

- Ubuntu 14.04
- Drupal 7.37
- MySQL 5.5.41
- Apache 2.26
- Tomcat 7.0.52
- Solr 4.10.3
- Camel 2.14.1
- Fedora 4.3.0
- Fedora Camel Component 4.2.0
- BlazeGraph 1.5.1
- Karaf 3.0.4
- Sync 0.0.0
- Islandora 7.x-2.x
- PHP 5.5.9 
- Java 8 (Oracle)

## Requirements

1. [Vagrant](http://www.vagrantup.com/)
2. [Virtualbox](https://www.virtualbox.org/)
3. [git](https://git-scm.com/)

## Initial Configuration

Before deploy the environment you must configure it. There are number of files which need to be populated with information all of which can be found in the [vagrant](/vagrant) directory.

Simply copy all of the example files such that they are no longer prefixed with *example.*, like so:

```bash
cd ./vagrant
cp example.aws.yml aws.yml
```

Or you can run the convenience script included in the [scripts](/scripts) folder to do this for you:

```bash
cd ./scripts
generate-default-configuration
```

This will be enough to start using the *Virtualbox* provider, but for other providers and for more advanced configuration read the documentation below.

### Shared Settings

Each provider will let you set the following settings, although they will behave slightly different on each environment. Please see Vagrant documentation on [Synced Folders](https://www.vagrantup.com/docs/synced-folders/) and [Port Forwarding](https://www.vagrantup.com/docs/networking/forwarded_ports.html) for more details.

* `synced_folders` - An associative array of folders to "sync" the following
  options.
  * `local_path` - The relative path locally to sync.
  * `destination` - The full path on the VM to sync to.
  * `excluded_paths` - Paths not to sync (__only works with rsync__)

* `port_mapping`
  * `guest` - The port on the guest to map from.
  * `host` - The port on the guest to map to.
  
*N.B.* Be aware only Virtualbox supports bi-directional syncing, all other
 providers use rsync.

## Provider Requirements: 

### Virtualbox

If you followed the steps above you should be ready to go. If required you can customize the amount of RAM / CPU's, the port mapping and which folders to sync.

This provider exposes quite a few provider-specific configuration options:

* `box` - The base Virtualbox image to build from.
* `cpus` - The number of virtual CPU's to allocate to the VM.
* `memory` - The number of MB to allocate for RAM.
* `name` - The name used for the Virtualbox VM, it must be unique.

### Digital Ocean

You must first install the Digital Ocean Vagrant plugin:

```bash
vagrant plugin install vagrant-digitalocean
```

This provider exposes quite a few provider-specific configuration options:

* `image` - The image to use when creating a new droplet.
* `name` - The name used to identify the Droplet, it must be unique.
* `private_key_path` - The path locally to your Digital Ocean SSH Key.
* `region` - The region to create the new droplet in.
* `size` - The size to use when creating a new droplet (e.g. `4gb`).
* `ssh_key_name` - The name of your Digital Ocean SSH Key.
* `token` -  Your Digital Ocean Personal Access Token.

### Digital Ocean, Additional Info

* List available images with the `vagrant digitalocean-list images $DIGITAL_OCEAN_TOKEN` command
* List available regions with the `vagrant digitalocean-list regions $DIGITAL_OCEAN_TOKEN` command.
* List available sizes with the `vagrant digitalocean*list sizes $DIGITAL_OCEAN_TOKEN` command.

### AWS

You must first install the AWS Vagrant plugin:

```bash
vagrant plugin install vagrant-aws
```

This provider exposes quite a few provider-specific configuration options:

* `access_key_id` - The access key for accessing AWS
* `ami` - The AMI id to boot, such as "ami-12345678"
* `instance_type` - The type of instance, such as "m3.medium". 
* `keypair_name` - The name of the keypair to use to bootstrap AMI's.
* `private_key_path` - The path locally to your Digital Ocean SSH Key.
* `region` - The region to start the instance in, such as "us-east-1"
* `secret_access_key` - The secret access key for accessing AWS
* `session_token` - The session token provided by STS


## Setup

Assuming you've done the appropriate configuration from above. Clone this repository locally, and choose a provider.

```bash
git clone https://github.com/islandora-claw/claw-ansible
cd claw-ansible
```

Boxes take approximately _30 mins_ to come up in the cloud, and it can take much longer locally depending on your internet connection.

### Provider: Virtualbox

```bash
$ vagrant up
```

### Provider: Digital Ocean

```bash
$ vagrant up --provider=digital_ocean
```

### Provider: AWS

```bash
$ vagrant up --provider=aws
```

*N.B.* You may not be able to connect to your AWS instance depending on VPC settings.

## View Site (Virtualbox)

Your selected provider and/or configuration will determine where the site is hosted, and at which port. Here we will only focus on Virtualbox with the default settings.

You can connect to the machine via the browser at [http://localhost:8000](http://localhost:8000).

The Fedora 4 REST API can be accessed at [http://localhost:8080/fcrepo/rest](http://localhost:8080/fcrepo/rest). It currently has authentication disabled.

## Credentials

You'll need credentials to access various parts of the site, as part of the deployment process automatic passwords are generated, this is to protect you from attacks when the box is accessible on the internet.

You'll find them in the *secrets.yml* file in this repository, it is generated automatically when you run vagrant up.

## Maintainers/Sponsors

* UPEI
* discoverygarden inc.
* LYRASIS
* McMaster University
* University of Limerick
* York University
* University of Manitoba
* Simon Fraser University
* PALS
* American Philosophical Society
* common media inc.

Current maintainers:

* [Nigel Banks](https://github.com/nigelgbanks)
* [Nick Ruest](https://github.com/ruebot)

## Development

If you would like to contribute, please get involved by attending our weekly [Tech Call](https://github.com/Islandora-CLAW/CLAW/wiki). We love to hear from you!

If you would like to contribute code to the project, you need to be covered by an Islandora Foundation [Contributor License Agreement](http://islandora.ca/sites/default/files/islandora_cla.pdf) or [Corporate Contributor Licencse Agreement](http://islandora.ca/sites/default/files/islandora_ccla.pdf). Please see the [Contributors](http://islandora.ca/resources/contributors) pages on Islandora.ca for more information.

## License

[MIT](https://opensource.org/licenses/MIT)
