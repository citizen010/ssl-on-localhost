![MIT license](https://img.shields.io/badge/license-MIT-blue)

# How to make SSL working on localhost ##

## About ##

This HOW-TO has been succesfully tested on Ubuntu 19.10 LTS with nginx v. 1.16.1 so let's assume you have a similar setup.

Using certificates from real certificate authorities (CAs) for development can be dangerous or impossible (for hosts like `localhost` or `127.0.0.1`),  but self-signed certificates cause trust errors. Managing your own CA is the best solution, but usually involves arcane commands, specialized knowledge and manual steps.

[mkcert](https://github.com/FiloSottile/mkcert) is a GitHub project maintained by Filippo Valsorda and is a simple tool for making locally-trusted development certificates. It  automatically creates and installs a local CA in the system root store and generates locally-trusted certificates.

> Remember that **mkcert** is meant for development purposes, not production,  so it should not be used on end users machines, and that you should not export or share `rootCA-key.pem`.

## Installation ##

Make sure you're logged in as a regular user (_not as root_).

First install `certutil`

`sudo apt install libnss3-tools`

Even you can build it from source, I suggest to download directly the [pre-built binary](https://github.com/FiloSottile/mkcert/releases) for Linux on your home directory, make it executable and move it to a path like `/usr/local/bin`  while renaming it as **mkcert**

```
sudo wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.1/mkcert-v1.4.1-linux-amd64

sudo chmod +x mkcert-v1.4.1-linux-amd64

sudo mv ./mkcert-v1.4.1-linux-amd64 /usr/local/bin/mkcert
```
## Generate a local CA ##

```
mkcert -install
Created a new local CA at "/home/YOURUSERNAME/.local/share/mkcert" 💥
The local CA is now installed in the system trust store! ⚡️
The local CA is now installed in the Firefox trust store (requires restart)!🦊
```
> Warning: the `rootCA-key.pem` file that mkcert automatically generates gives complete power to intercept secure requests from your machine. Do not share it.

## Generate a certificate for localhost ##

```
mkcert localhost 127.0.0.1
Using the local CA at "/home/YOURUSERNAME/.local/.share/mkcert" ✨

Created a new certificate valid for the following names 📜
 - "localhost"
 - "127.0.0.1"

The certificate is at "./localhost+1.pem" and the key at "./localhost+1-key.pem" ✅
```
> You should be able to generate certificates also for local domains (__eg: myapp.dev, testdomain.app, etc.__) assuming that you have a DNS on local network able to resolve those names, but this is beyond the scope of this tutorial. You can find more info on the [GitHub page](https://github.com/FiloSottile/mkcert) of the project.

## Configuring nginx ##

Due that mkcert does not automatically configure servers to use the certificates, let's make some nginx configuration.

`sudo nano /etc/nginx/sites-enabled/default`

Whit your preferred editor, edit the file above as it looks like this (__be sure to replace the values to match your setup__):

```
server {
	listen localhost:443 ssl;
	listen 127.0.0.1:443 ssl;

	ssl_certificate        /home/YOURUSERNAME/localhost+1.pem;
	ssl_certificate_key    /home/YOURUSERNAME/localhost+1-key.pem;

	server_name localhost;
	access_log /var/log/nginx/localhost.access.log;
	error_log /var/log/nginx/localhost.error.log;
	location / {
		root   /var/www/html/;
		index  index.html;
	}
}
```

## Testing nginx configuration ##

```
sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

## Restarting nginx ##

`sudo service nginx restart`

## Conclusion ##

Make sure you have an `index.html` file with some content on `/var/wwww/html/` and, if all went good, you can enjoy your secure site at https://localhost.

## License ##
<a href="https://raw.githubusercontent.com/citizen010/empty-site-template/master/LICENSE" rel="nofollow"><img src="https://camo.githubusercontent.com/890acbdcb87868b382af9a4b1fac507b9659d9bf/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d626c75652e737667" alt="GitHub license" data-canonical-src="https://img.shields.io/badge/license-MIT-blue.svg" style="max-width:100%;"></a>
