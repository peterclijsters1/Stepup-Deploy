# Stepup development boxes

## Boxes

 * `stepup-apps` — Apache2, PHP-FPM, MariaDB and virtual hosts for Middleware, Gateway, Self-Service and RA;
 * `stepup-logging` — Graylog2 server and web interface.

## Requirements

 * VirtualBox (tested with 5.0.18)
 * Vagrant (tested with 1.8.4)
 * Vagrant vbguest plugin (instructions below)
 * Ansible (tested with 2.0.0.2), fallback to self-provisioning on Windows (untested)

## Installation

This setup is designed to run next to the four step-up applications Middleware, Gateway, Self-Service and RA and uses
a simplesamlphp installation to act as development IdP:

 * `Stepup-Deploy/` — Contains the application and Graylog2 boxes
 * `Stepup-Middleware/` — Contains the Middleware application code
 * `Stepup-Gateway/` — Contains the Gateway application code
 * `Stepup-SelfService/` — Contains the Self-Service application code
 * `Stepup-RA/` — Contains the RA application code
 * `simplesamlphp/` - Contains the simplesamlphp installation

```sh-session
$ mkdir stepup
$ cd stepup

$ git clone git@github.com:OpenConext/Stepup-Deploy.git
$ git clone git@github.com:OpenConext/Stepup-Middleware.git
$ git clone git@github.com:OpenConext/Stepup-Gateway.git
$ git clone git@github.com:OpenConext/Stepup-SelfService.git
$ git clone git@github.com:OpenConext/Stepup-RA.git
$ mkdir simplesamlphp # Place a [simplesamlphp deployment](https://github.com/simplesamlphp/simplesamlphp) here with some credentials and set the baseurlpath to "/"
```
__Install third party dependencies:__

Run composer install --ignore-platform-reqs from your host in the following projects:

* simplesamlphp
* Stepup-Gateway
* Stepup-Middleware
* Stepup-RA
* Stepup-SelfService
 
__Then continue with installation:__

```
$ cd Stepup-Deploy
$ mkdir ssl
$ (cd ssl && place-ssl-certificates-see-below)
$ git checkout dev
$ vagrant plugin install vagrant-vbguest
$ vagrant up
```

_Do note that the installation of graylog requires the downloading of a .deb. This can take some time._

Place the required SSL certificates in `./Stepup-Deploy/ssl`:

 * `ca.crt` — CA certificate chain
 * `server.crt` — Server wildcard certificate
 * `server.key` — Server wildcard private key

Edit your hosts file to include the following:

```
10.10.0.100  mw-dev.stepup.coin.surf.net gw-dev.stepup.coin.surf.net ss-dev.stepup.coin.surf.net ra-dev.stepup.coin.surf.net idp-dev.stepup.coin.surf.net
10.10.0.101  g2-dev.stepup.coin.surf.net
```

 * To view the self-service, visit https://ss-dev.stepup.coin.surf.net/app_dev.php/.
 * To view the Graylog2 web interface, visit http://g2-dev.stepup.coin.surf.net:9000 and use username `admin` and password `password`.
 * To view mails caught by Mailcatcher, visit http://mw-dev.stepup.coin.surf.net:1080/

To configure the applications for development, use the [development documentation][dev-docs].

[dev-docs]: ./docs/development.md
