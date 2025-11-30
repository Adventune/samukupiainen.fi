---
title: Setting up docker registry access control
description: 'Other online guides only go through setting up a reverse proxy. Lets do better!'
heroImage: './container-port.jpg'
pubDate: 'Jan 19 2023'
---

While I was setting up a Docker registry, I noticed that lot of the online guides only go through
methods of setting up a reverse proxy with basic auth or something along those lines for access
control. That does not sound so practical..

I found a great tool for achieving more granular access control:
[Cesanta's docker_auth](https://github.com/cesanta/docker_auth/) and basic setup for it.

### What is docker_auth?

Docker_auth is a token based authentication and authorization server for docker registries. Below is
the begginnings of the README.md from the project.

> The original Docker Registry server (v1) did not provide any support for authentication or
> authorization. Access control had to be performed externally, typically by deploying Nginx in the
> reverse proxy mode with Basic or other type of authentication. While performing simple user
> authentication is pretty straightforward, performing more fine-grained access control was
> cumbersome. <br/><br/> Docker Registry 2.0 introduced a new, token-based authentication and
> authorization protocol, but the server to generate them was not released. Thus, most guides found
> on the internet still describe a set up with a reverse proxy performing access control. <br/><br/>
> This server fills the gap...

We can use docker_auth as a server to grant access to multiple different docker registries and with
user specific access control. It also supports multiple different authentication backends like LDAP
and OAuth2.

### Setting up a docker registry with docker_auth

#### Docker compose file

I will be using docker-compose to set up docker_auth and a docker registry. I always have preffered
the compose workflow over running docker cli commands.

```yaml
version: '3.8'

services:
    auth:
        image: cesanta/docker_auth
        ports:
            - '5001:5001'
        volumes:
            - ./auth-config:/config
        restart: unless-stopped

    registry:
        image: registry
        ports:
            - '5000:5000'
        environment:
            - REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY=/data
        volumes:
            - ./registry-data:/data
        restart: unless-stopped
        depends_on:
            - auth
```

With this setup we will have both services running on the same host. They can be separated to
different hosts if needed.

#### Auth server configuration

Lets create the `auth-config` folder and `auth_config.yml` file inside it. We will be using a
pre-defined users list for authentication.

```yaml
token:
    issuer: 'Acme auth server' # Must mach the issuer in the registry config
    expiration: 900
    # If you are not using a reverse proxy specifying the certificate and key here will not be necessary.
    # If you do not specify them here, server certificate and key will be used.
    certificate: '/config/auth.pem'
    key: '/config/auth.key'

server:
    addr: ':5001'
    # I have set up a reverse proxy in front of the auth server and TLS termination is done there.
    # If you are not using a reverse proxy, you can uncomment the following lines to enable TLS.
    # certificate: "/path/to/server.pem"
    # key: "/path/to/server.key"

users:
    # Password is specified as a BCrypt hash. Use `htpasswd -nB USERNAME` to generate.
    'admin':
        password: '$2y$05$LO.vzwpWC5LZGqThvEfznu8qhb5SGqvBSWY1J3yZ4AxtMRZ3kN5jC' # badmin
    '': {} # Anonymous user

acl: # Access control list
    - match: { account: 'admin' }
      actions: ['*']
      comment: 'Admin has full access to everything.'
    - match: { account: '', name: 'hello-world' }
      actions: ['pull']
      comment: 'Anonymous users can pull the "hello-world" image.'
```

If some of the configuration options are unclear, you can find the full reference in the
[docker_auth repository](https://github.com/cesanta/docker_auth/blob/main/examples/reference.yml). I
will go through the cert and key generation in a later section.

Now we have our auth server pretty much setup (apart from the cert and key). Lets move on to the
registry configuration.

#### Registry auth configuration

To enable authentication for the registry, following environment variables need to be set.

```yaml
services:
    registry:
        environment:
            - REGISTRY_AUTH=token
            - REGISTRY_AUTH_TOKEN_REALM="http://127.0.0.1:5001/auth" # Disclaimer: Auth URL is relative to the user not the registry
            - REGISTRY_AUTH_TOKEN_ISSUER="Acme auth server" # Must match the issuer in the auth server config
            - REGISTRY_AUTH_TOKEN_SERVICE="registry"
            - REGISTRY_AUTH_TOKEN_ROOTCERTBUNDLE="/auth/certs/auth.pem" # Path to the auth server certificate
```

Also add the following volume to the registry service.

```yaml
services:
    registry:
        volumes:
            - ./auth-config/auth.pem:/auth/certs/auth.pem
```

Now we have our registry setup to use the auth server. Auth token realm will be the URL where the
auth server is located. Note that the caller is not the registry but the user. For more details
check out the
[registry token auth specification](https://distribution.github.io/distribution/spec/auth/token/).

#### Generating the certificate and key

We are now almost done with our registry and auth server setup. We just need to generate the cert
and key for the authentication. Key generation consists of creating a private key and a certificate.
For self-signed certificates you can use the following commands.

```bash
# Generate private key
openssl genpkey -algorithm RSA -out ./auth-config/auth.key

# Create a certificate signing request
openssl req -new -key ./auth-config/auth.key -out csr.pem # Answer the questions for the self signed certificate

# Generate the certificate
openssl x509 -req -in csr.pem -signkey ./auth-config/auth.key -out ./auth-config/auth.pem
```

#### Testing the setup

Now we have everything in place to spin up a registry and auth server. Just run `docker compose up`
and you should have a working registry with access control. For other than pre-defined user
authentication check out the
[docker_auth documentation](https://github.com/cesanta/docker_auth/tree/main/docs).

### Conclusion

I hope this post finds someone who is looking for this information. It's a shame that there are not
more guides on the topic. If you want to check out the full code for this post, you can find it in
[my github](https://github.com/Adventune/docker-registry-access-control).
