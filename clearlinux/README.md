<!--

********************************************************************************

WARNING:

    DO NOT EDIT "clearlinux/README.md"

    IT IS AUTO-GENERATED

    (from the other files in "clearlinux/" combined with a set of templates)

********************************************************************************

-->

# **DEPRECATION NOTICE**

https://community.clearlinux.org/t/all-good-things-come-to-an-end-shutting-down-clear-linux-os/10716

> Effective immediately [2025-07-18], Intel will no longer provide security patches, updates, or maintenance for Clear Linux OS, and the Clear Linux OS GitHub repository will be archived in read-only mode. So, if you’re currently using Clear Linux OS, we strongly recommend planning your migration to another actively maintained Linux distribution as soon as possible to ensure ongoing security and stability.

# Quick reference

-	**Maintained by**:  
	[Intel Corporation](https://github.com/clearlinux/docker-brew-clearlinux)

-	**Where to get help**:  
	[the Docker Community Slack](https://dockr.ly/comm-slack), [Server Fault](https://serverfault.com/help/on-topic), [Unix & Linux](https://unix.stackexchange.com/help/on-topic), or [Stack Overflow](https://stackoverflow.com/help/on-topic)

# Supported tags and respective `Dockerfile` links

-	[`latest`, `base`](https://github.com/clearlinux/docker-brew-clearlinux/blob/03e2dcd390233733b398e72ce2382e54b5ef48a4/Dockerfile)

# Quick reference (cont.)

-	**Where to file issues**:  
	[https://github.com/clearlinux/docker-brew-clearlinux/issues](https://github.com/clearlinux/docker-brew-clearlinux/issues?q=)

-	**Supported architectures**: ([more info](https://github.com/docker-library/official-images#architectures-other-than-amd64))  
	[`amd64`](https://hub.docker.com/r/amd64/clearlinux/)

-	**Published image artifact details**:  
	[repo-info repo's `repos/clearlinux/` directory](https://github.com/docker-library/repo-info/blob/master/repos/clearlinux) ([history](https://github.com/docker-library/repo-info/commits/master/repos/clearlinux))  
	(image metadata, transfer size, etc)

-	**Image updates**:  
	[official-images repo's `library/clearlinux` label](https://github.com/docker-library/official-images/issues?q=label%3Alibrary%2Fclearlinux)  
	[official-images repo's `library/clearlinux` file](https://github.com/docker-library/official-images/blob/master/library/clearlinux) ([history](https://github.com/docker-library/official-images/commits/master/library/clearlinux))

-	**Source of this description**:  
	[docs repo's `clearlinux/` directory](https://github.com/docker-library/docs/tree/master/clearlinux) ([history](https://github.com/docker-library/docs/commits/master/clearlinux))

# Clear Linux OS

This serves as the official [Clear Linux OS](https://clearlinux.org) image.

![logo](https://raw.githubusercontent.com/docker-library/docs/dbe1941be63c87cc691b59d50f830f9dd7d69df9/clearlinux/logo.png)

The `clearlinux:latest` tag will point to `clearlinux:base` which will track toward the latest release version of the distribution.

This image contains the os-core and os-core-update bundles, the latter can be used to add additional Clear Linux OS components (see [here](https://clearlinux.org/documentation/swupdate_about_sw_update.html) for more details about swupd and [here](https://clearlinux.org/documentation/bundles_overview.html) for more information on bundles).

The following Dockerfile will install the editors and dev-utils bundles on top of the base image

```sh
FROM clearlinux:base
RUN swupd bundle-add editors dev-utils
```

Where editors contains the usual suspects for command line editors and dev-utils contains some handy development tools like strace, gdb and valgrind.

# License

View [Clear Linux OS for Intel Architecture](https://download.clearlinux.org/current/licenses) for the licenses of the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [the `repo-info` repository's `clearlinux/` directory](https://github.com/docker-library/repo-info/tree/master/repos/clearlinux).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
