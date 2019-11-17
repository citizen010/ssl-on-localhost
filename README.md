# How to make SSL working on localhost ##

## Abstract ##

This HOW-TO has been succesfully tested on Ubuntu 19.10 LTS with nginx v. 1.16.1 so let's assume you have a similar setup.

Using certificates from real certificate authorities (CAs) for development can be dangerous or impossible (for hosts like `localhost` or `127.0.0.1`),  but self-signed certificates cause trust errors. Managing your own CA is the best solution, but usually involves arcane commands, specialized knowledge and manual steps.

[mkcert](https://github.com/FiloSottile/mkcert) is a GitHub project maintained by Filippo Valsorda and is a simple tool for making locally-trusted development certificates. It  automatically creates and installs a local CA in the system root store and generates locally-trusted certificates.

> Remember that mkcert is meant for development purposes, not production,  so it should not be used on end users machines, and that you should not export or share `rootCA-key.pem`.

## Installation ##

Make sure you're logged in as a regular user (_not as root_).
<<<<<<< HEAD
Even you can build it from source, I suggest to download directly the [pre-built binary](https://github.com/FiloSottile/mkcert/releases) for Linux on your home directory, make it executable and move it to a path like ՝/usr/local/bin՝  while renaming it as **mkcert**
=======
Even you can build it from source, I suggest to download directly the [pre-built binary](https://github.com/FiloSottile/mkcert/releases) for Linux on your home directory, make it executable and move it to a path like `/usr/local/bin`  while renaming it as **mkcert**

```
wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.0/mkcert-v1.4.0-linux-amd64

chmod +x mkcert-v1.4.0-linux-amd64

sudo mv ./mkcert-v1.4.0-linux-amd64 /usr/local/bin/mkcert
```
