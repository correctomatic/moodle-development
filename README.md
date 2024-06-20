# Moodle development environment

Moodle development environment, aimed mainly at developers who want to create LTI tools.

## Launching the environment

You will need a copy of the Moodle code in the `moodle` directory. You can clone the repository with the following command:

```sh
git clone -b v<moodle version> https://github.com/moodle/moodle.git moodle
```

You will also need to use on your host the DNS server provided by the environment. The DNS server exposes port `3535`. It resolves the `*.lti` domain to the development containers, and redirects the other requests to `8.8.8.8` by default. Alternatively, you can modify your host's host file adding the following entries:

```conf
# -------------------------------------------
# LTI application development entries
# -------------------------------------------
172.20.0.5  moodle.lti
172.20.0.10 app.lti
172.20.0.1 app.host.lti
```

After that, bring up the containers:
```bash
docker-compose up
```

Then follow the Moodle installation process at `http://moodle.lti`. The database credentials are:
- Server: `postgress.lti`
- User: `moodle`
- Password: `moodle`
- Database: `moodle`

## Containers

The environment will create the following containers:
- `lti_dns`: DNS server. More on this later.
- `lti_nginx` (DNS record: `moodle.lti`): Web server, redirects requests to the PHP-FPM container.
- `lti_php` (DNS record: `php.lti`): PHP-FPM container, serving the Moodle code.
- `lti_postgres` (DNS record: `postgres.lti`): PostgreSQL database.

The environment also creates a network called `lti_network`. All containers are connected to this network, and have
fixed IP addresses. The address `172.20.0.10` is reserved for the LTI development server that you will use with this environment.

## Developing a LTI tool

Developing a LTI app is tricky, because:
- Your browser needs to launch requests to the Moodle server and to the LTI app.
- The LTI app needs to launch requests to the Moodle server.
- The Moodle server needs to launch requests to the LTI app.

Also, the Moodle's server must have the same domain from the LTI app perspective and from your host perspective, and the same applies to the LTI app.

To solve this, we have a DNS server that resolves the `*.lti` domain to the development containers. This way, you can use the same domain in your LTI app and in Moodle.

**Important**: remember to configure the name resolution properly, as explained in the "Launching the environment" section.

### Running the LTI app in your host

If you want to run your LTI app in your host, you will need to use the hostname `app.host.lti`. This hostname is resolved to the IP address of your host in the Docker network.

### Running the LTI app in a Docker container

If you want to run your LTI app in a Docker container, you will need to add the container to the `lti_network` network and assign the DNS server to that container. You will also need to use the hostname `app.lti` when configuring the LTI tool in Moodle.

This is an example configuration for a LTI tool written in Python, exposing the application on port `5000` and the debugger in port `5678`:

```dockerfile
# Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY app/requirements.txt requirements.txt
RUN pip install -r requirements.txt

# Needed for remote debugging (Python Tools for Visual Studio Debugging)
RUN pip install debugpy

# Debug tools
RUN apt-get update \
    && apt-get install -y procps iputils-ping curl \
    && echo "alias ll='ls -la'" >> /root/.bashrc

CMD ["python", "app.py"]
```

```yaml
# docker-compose.yml
services:

  app:
    build: .
    container_name: lti_app
    volumes:
      - ./app:/app
    ports:
      - "5000:5000"
      - "5678:5678"  # vscode debugger
    networks:
      lti_network:
        # This address is defined in the moodle's dns container
        # in the file dnsmasq.conf. This MUST be the same address.
        ipv4_address: 172.20.0.10
    dns: 172.20.0.100

networks:
  # This is the name of the network used by the moodle's docker-compose.yml
  # The app MUST be in the same network as the moodle, because moodle
  # makes requests to the app from moodle's container
  lti_network:
    external: true
```

```python
# Python app
import debugpy

# ....
debugpy.listen(("0.0.0.0", 5678))
# ....
```


## Debugging Moodle

Moodle container has Xdebug installed and configured. It exposes it on port `9003`. You have an example configuration for VSCode included in the `vscode` directory.

