<!--

********************************************************************************

WARNING:

    DO NOT EDIT "emqx/README.md"

    IT IS AUTO-GENERATED

    (from the other files in "emqx/" combined with a set of templates)

********************************************************************************

-->

# **DEPRECATION NOTICE**

Starting from v5.9.0, EMQX has unified all features from the previous Open Source and Enterprise editions into a single, powerful offering with the Business Source License (BSL) 1.1.

If you want to understand why we made the change, please read this [blog post](https://www.emqx.com/en/news/emqx-adopts-business-source-license), and if you want to know more about the new license, please read the [EMQX Licensing FAQ](https://www.emqx.com/en/content/license-faq).

Consequently, we stopped publishing the `emqx` Docker Official Image. EMQX v5.9.0+ will only be available in the [`emqx/emqx`](https://hub.docker.com/r/emqx/emqx) and [`emqx/emqx-enterprise`](https://hub.docker.com/r/emqx/emqx-enterprise) Docker Hub repositories.

# Quick reference

-	**Maintained by**:  
	[EMQ Technologies](https://github.com/emqx)

-	**Where to get help**:  
	[Discussions](https://github.com/emqx/emqx/discussions) or [Discord](https://discord.gg/xYGf3fQnES)

# Supported tags and respective `Dockerfile` links

-	[`5.7.2`, `5.7`](https://github.com/emqx/emqx-docker/blob/35e70c8e602687db5a447c9573bde8ab77335fdc/5.7/Dockerfile)

-	[`5.8.7`, `5.8`, `5`, `latest`](https://github.com/emqx/emqx-docker/blob/18bad90efc25023480d0a11978f7d90e085116b5/5.8/Dockerfile)

# Quick reference (cont.)

-	**Where to file issues**:  
	[https://github.com/emqx/emqx-docker/issues](https://github.com/emqx/emqx-docker/issues?q=)

-	**Supported architectures**: ([more info](https://github.com/docker-library/official-images#architectures-other-than-amd64))  
	[`amd64`](https://hub.docker.com/r/amd64/emqx/), [`arm64v8`](https://hub.docker.com/r/arm64v8/emqx/)

-	**Published image artifact details**:  
	[repo-info repo's `repos/emqx/` directory](https://github.com/docker-library/repo-info/blob/master/repos/emqx) ([history](https://github.com/docker-library/repo-info/commits/master/repos/emqx))  
	(image metadata, transfer size, etc)

-	**Image updates**:  
	[official-images repo's `library/emqx` label](https://github.com/docker-library/official-images/issues?q=label%3Alibrary%2Femqx)  
	[official-images repo's `library/emqx` file](https://github.com/docker-library/official-images/blob/master/library/emqx) ([history](https://github.com/docker-library/official-images/commits/master/library/emqx))

-	**Source of this description**:  
	[docs repo's `emqx/` directory](https://github.com/docker-library/docs/tree/master/emqx) ([history](https://github.com/docker-library/docs/commits/master/emqx))

# What is EMQX

[EMQX](https://emqx.io/) is the world's most scalable open-source MQTT broker with a high performance that connects 100M+ IoT devices in 1 cluster, while maintaining 1M message per second throughput and sub-millisecond latency.

EMQX supports multiple open standard protocols like MQTT, HTTP, QUIC, and WebSocket. It's 100% compliant with MQTT 5.0 and 3.x standard, and secures bi-directional communication with MQTT over TLS/SSL and various authentication mechanisms.

With the built-in powerful SQL-based rules engine, EMQX can extract, filter, enrich and transform IoT data in real-time. In addition, it ensures high availability and horizontal scalability with a masterless distributed architecture, and provides ops-friendly user experience and great observability.

EMQX boasts more than 20K+ enterprise users across 50+ countries and regions, connecting 100M+ IoT devices worldwide, and is trusted by over 400 customers in mission-critical scenarios of IoT, IIoT, connected vehicles, and more, including over 70 Fortune 500 companies like HPE, VMware, Verifone, SAIC Volkswagen, and Ericsson.

![logo](https://raw.githubusercontent.com/docker-library/docs/68aa4264fa058f323993fdaceacd63a8acbbeb48/emqx/logo.svg?sanitize=true)

# How to use this image

### Run EMQX

Execute some command under this docker image

```console
$ docker run -d --name emqx emqx:${tag}
```

For example

```console
$ docker run -d --name emqx -p 18083:18083 -p 1883:1883 emqx:latest
```

The EMQX broker runs as Linux user `emqx` in the docker container.

### Configuration

All EMQX Configuration in [`etc/emqx.conf`](https://github.com/emqx/emqx/blob/master/apps/emqx/etc/emqx.conf) can be configured via environment variables.

Example:

	EMQX_DASHBOARD__DEFAULT_PASSWORD       <--> dashboard.default_password
	EMQX_NODE__COOKIE                      <--> node.cookie
	EMQX_LISTENERS__SSL__default__ENABLE   <--> listeners.ssl.default.enable

Note: The lowercase use of 'default' is not a typo. It is used to demonstrate that lowercase environment variables are equivalent.

-	Prefix `EMQX_` is removed
-	All upper case letters are replaced with lower case letters
-	`__` is replaced with `.`

For example, set MQTT TCP port to 1883

```console
$ docker run -d --name emqx -e EMQX_DASHBOARD__DEFAULT_PASSWORD=mysecret -p 18083:18083 -p 1883:1883 emqx:latest
```

Please read more about EMQX configuration in the [official documentation](https://docs.emqx.com/en/emqx/latest/configuration/configuration.html)

#### EMQX node name configuration

Environment variable `EMQX_NODE__NAME` allows you to specify an EMQX node name, which defaults to `<container_name>@<container_ip>`.

If not specified, EMQX determines its node name based on the running environment or other environment variables used for node discovery.

### Cluster

EMQX supports a variety of clustering methods, see our [documentation](https://docs.emqx.com/en/emqx/latest/deploy/cluster/create-cluster.html) for details.

Let's create a static node list cluster from Docker Compose.

-	Create `compose.yaml`:

```yaml
services:
  emqx1:
    image: emqx:latest
    environment:
    - "EMQX_NODE__NAME=emqx@node1.emqx.io"
    - "EMQX_CLUSTER__DISCOVERY_STRATEGY=static"
    - "EMQX_CLUSTER__STATIC__SEEDS=[emqx@node1.emqx.io, emqx@node2.emqx.io]"
    networks:
      emqx-bridge:
        aliases:
        - node1.emqx.io

  emqx2:
    image: emqx:latest
    environment:
    - "EMQX_NODE__NAME=emqx@node2.emqx.io"
    - "EMQX_CLUSTER__DISCOVERY_STRATEGY=static"
    - "EMQX_CLUSTER__STATIC__SEEDS=[emqx@node1.emqx.io, emqx@node2.emqx.io]"
    networks:
      emqx-bridge:
        aliases:
        - node2.emqx.io

networks:
  emqx-bridge:
    driver: bridge
```

-	Start the Docker Compose services

```bash
docker compose -p my_emqx up -d
```

-	View cluster

```bash
$ docker exec -it my_emqx_emqx1_1 sh -c "emqx ctl cluster status"
Cluster status: #{running_nodes => ['emqx@node1.emqx.io','emqx@node2.emqx.io'],
                  stopped_nodes => []}
```

### Persistence

If you want to persist the EMQX docker container, you need to keep the following directories:

-	`/opt/emqx/data`
-	`/opt/emqx/log`

Since data in these folders are partially stored under the `/opt/emqx/data/mnesia/${node_name}`, the user also needs to reuse the same node name to see the previous state. To make this work, one needs to set the host part of `EMQX_NODE__NAME` to something static that does not change when you restart or recreate the container. It could be container name, hostname or loopback IP address `127.0.0.1` if you only have one node.

In if you use Docker Compose, the configuration would look something like this:

```YAML
volumes:
  vol-emqx-data:
    name: foo-emqx-data
  vol-emqx-log:
    name: foo-emqx-log

services:
  emqx:
    image: emqx:latest
    restart: always
    environment:
      EMQX_NODE__NAME: foo_emqx@127.0.0.1
    volumes:
      - vol-emqx-data:/opt/emqx/data
      - vol-emqx-log:/opt/emqx/log
```

### Kernel Tuning

Under Linux host machine, the easiest way is [Tuning guide](https://docs.emqx.com/en/emqx/latest/performance/tune.html).

If you want tune Linux kernel by docker, you must ensure your docker is latest version (>=1.12).

```bash
docker run -d --name emqx -p 18083:18083 -p 1883:1883 \
    --sysctl fs.file-max=2097152 \
    --sysctl fs.nr_open=2097152 \
    --sysctl net.core.somaxconn=32768 \
    --sysctl net.ipv4.tcp_max_syn_backlog=16384 \
    --sysctl net.core.netdev_max_backlog=16384 \
    --sysctl net.ipv4.ip_local_port_range=1000 65535 \
    --sysctl net.core.rmem_default=262144 \
    --sysctl net.core.wmem_default=262144 \
    --sysctl net.core.rmem_max=16777216 \
    --sysctl net.core.wmem_max=16777216 \
    --sysctl net.core.optmem_max=16777216 \
    --sysctl net.ipv4.tcp_rmem=1024 4096 16777216 \
    --sysctl net.ipv4.tcp_wmem=1024 4096 16777216 \
    --sysctl net.ipv4.tcp_max_tw_buckets=1048576 \
    --sysctl net.ipv4.tcp_fin_timeout=15 \
    emqx:latest
```

> REMEMBER: DO NOT RUN EMQX DOCKER PRIVILEGED OR MOUNT SYSTEM PROC IN CONTAINER TO TUNE LINUX KERNEL, IT IS UNSAFE.

# License

View [license information](https://github.com/emqx/emqx/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [the `repo-info` repository's `emqx/` directory](https://github.com/docker-library/repo-info/tree/master/repos/emqx).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
