# Percona Server for MySQL

Percona Server for MySQL is a fork of the MySQL relational database management system created by Percona.

It aims to retain close compatibility to the official MySQL releases, while focusing on performance and increased visibility into server operations. Also included in Percona Server is XtraDB, Percona's fork of the InnoDB Storage Engine.

> [wikipedia.org/wiki/Percona_Server](https://en.wikipedia.org/wiki/Percona_Server)

%%LOGO%%

# How to use this image

## Start a `%%IMAGE%%` server instance

Starting a Percona Server for MySQL instance is simple:

```console
$ docker run --name some-%%REPO%% -e MYSQL_ROOT_PASSWORD=my-secret-pw -d %%IMAGE%%:tag
```

... where `some-%%REPO%%` is the name you want to assign to your container, `my-secret-pw` is the password to be set for the MySQL root user and `tag` is the tag specifying the MySQL version you want. See the list above for relevant tags.

## Connect to Percona Server from the MySQL command line client

The following command starts another `%%IMAGE%%` container instance and runs the `mysql` command line client against your original `%%IMAGE%%` container, allowing you to execute SQL statements against your database instance:

```console
$ docker run -it --network some-network --rm %%IMAGE%% mysql -hsome-%%REPO%% -uexample-user -p
```

... where `some-%%REPO%%` is the name of your original `%%IMAGE%%` container (connected to the `some-network` Docker network).

This image can also be used as a client for non-Docker or remote instances:

```console
$ docker run -it --rm %%IMAGE%% mysql -hsome.mysql.host -usome-mysql-user -p
```

More information about the MySQL command line client can be found in the [MySQL documentation](http://dev.mysql.com/doc/en/mysql.html)

## %%COMPOSE%%

Run `docker compose up`, wait for it to initialize completely, and visit `http://localhost:8080` or `http://host-ip:8080` (as appropriate).

## Container shell access and viewing MySQL logs

The `docker exec` command allows you to run commands inside a Docker container. The following command line will give you a bash shell inside your `%%IMAGE%%` container:

```console
$ docker exec -it some-%%REPO%% bash
```

The log is available through Docker's container log:

```console
$ docker logs some-%%REPO%%
```

## Using a custom MySQL configuration file

The startup configuration is specified in the file `/etc/my.cnf`, and that file in turn includes any files found in the `/etc/my.cnf.d` directory that end with `.cnf`. Settings in files in this directory will augment and/or override settings in `/etc/my.cnf`. If you want to use a customized MySQL configuration, you can create your alternative configuration file in a directory on the host machine and then mount that directory location as `/etc/my.cnf.d` inside the `%%IMAGE%%` container.

If `/my/custom/config-file.cnf` is the path and name of your custom configuration file, you can start your `%%IMAGE%%` container like this (note that only the directory path of the custom config file is used in this command):

```console
$ docker run --name some-%%REPO%% -v /my/custom:/etc/my.cnf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d %%IMAGE%%:tag
```

This will start a new container `some-%%REPO%%` where the Percona Server for MySQL instance uses the combined startup settings from `/etc/my.cnf` and `/etc/my.cnf.d/config-file.cnf`, with settings from the latter taking precedence.

### Configuration without a `cnf` file

Many configuration options can be passed as flags to `mysqld`. This will give you the flexibility to customize the container without needing a `cnf` file. For example, if you want to change the default encoding and collation for all tables to use UTF-8 (`utf8mb4`) just run the following:

```console
$ docker run --name some-%%REPO%% -e MYSQL_ROOT_PASSWORD=my-secret-pw -d %%IMAGE%%:tag --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

If you would like to see a complete list of available options, just run:

```console
$ docker run -it --rm %%IMAGE%%:tag --verbose --help
```

## Environment Variables

When you start the `%%IMAGE%%` image, you can adjust the configuration of the instance by passing one or more environment variables on the `docker run` command line. Do note that none of the variables below will have any effect if you start the container with a data directory that already contains a database: any pre-existing database will always be left untouched on container startup.

### `MYSQL_ROOT_PASSWORD`

This variable is mandatory and specifies the password that will be set for the `root` superuser account. In the above example, it was set to `my-secret-pw`.

### `MYSQL_ROOT_HOST`

By default, `root` can connect from anywhere. This option restricts root connections to be from the specified host only. Also `localhost` can be used here for the local-only root access.

### `MYSQL_DATABASE`

This variable is optional and allows you to specify the name of a database to be created on image startup. If a user/password was supplied (see below) then that user will be granted superuser access ([corresponding to `GRANT ALL`](https://dev.mysql.com/doc/refman/en/creating-accounts.html)) to this database.

### `MYSQL_USER`, `MYSQL_PASSWORD`

These variables are optional, used in conjunction to create a new user and to set that user's password. This user will be granted superuser permissions (see above) for the database specified by the `MYSQL_DATABASE` variable. Both variables are required for a user to be created.

Do note that there is no need to use this mechanism to create the root superuser, that user gets created by default with the password specified by the `MYSQL_ROOT_PASSWORD` variable.

### `MYSQL_ALLOW_EMPTY_PASSWORD`

This is an optional variable. Set to `yes` to allow the container to be started with a blank password for the root user. *NOTE*: Setting this variable to `yes` is not recommended unless you really know what you are doing, since this will leave your instance completely unprotected, allowing anyone to gain complete superuser access.

### `MYSQL_RANDOM_ROOT_PASSWORD`

This is an optional variable. Set to `yes` to generate a random initial password for the root user (using `pwmake`). The generated root password will be printed to stdout (`GENERATED ROOT PASSWORD: .....`).

### `MYSQL_ONETIME_PASSWORD`

Sets root (*not* the user specified in `MYSQL_USER`!) user as expired once init is complete, forcing a password change on first login. *NOTE*: This feature is supported on MySQL 5.6+ only. Using this option on MySQL 5.5 will throw an appropriate error during initialization.

### `MYSQL_INITDB_SKIP_TZINFO`

At first run MySQL automatically loads from the local system the timezone information needed for the `CONVERT_TZ()` function. If it's is not what is intended, this option disables timezone loading.

### `INIT_TOKUDB`

Tuns on TokuDB Engine. It can be activated only when *transparent huge pages* (THP) are disabled.

### `INIT_ROCKSDB`

Tuns on RocksDB Engine.

## Docker Secrets

As an alternative to passing sensitive information via environment variables, `_FILE` may be appended to the previously listed environment variables, causing the initialization script to load the values for those variables from files present in the container. In particular, this can be used to load passwords from Docker secrets stored in `/run/secrets/<secret_name>` files. For example:

```console
$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD_FILE=/run/secrets/mysql-root -d %%IMAGE%%:tag
```

Currently, this is only supported for `MYSQL_ROOT_PASSWORD`, `MYSQL_ROOT_HOST`, `MYSQL_DATABASE`, `MYSQL_USER`, and `MYSQL_PASSWORD`.

## Telemetry

Starting with Percona Server 8.0.35-27, telemetry will be enabled by default. If you decide not to send usage data to Percona, you can set the `PERCONA_TELEMETRY_DISABLE=1` environment variable. For example:

```console
$ docker run --name some-mysql -e  PERCONA_TELEMETRY_DISABLE=1 -d %%IMAGE%%:tag
```

# Initializing a fresh instance

When a container is started for the first time, a new database with the specified name will be created and initialized with the provided configuration variables. Furthermore, it will execute files with extensions `.sh`, `.sql` and `.sql.gz` that are found in `/docker-entrypoint-initdb.d`. Files will be executed in alphabetical order. You can easily populate your `%%IMAGE%%` services by [mounting a SQL dump into that directory](https://docs.docker.com/storage/bind-mounts/) and provide [custom images](https://docs.docker.com/reference/dockerfile/) with contributed data. SQL files will be imported by default to the database specified by the `MYSQL_DATABASE` variable.

# Caveats

## Where to Store Data

Important note: There are several ways to store data used by applications that run in Docker containers. We encourage users of the `%%IMAGE%%` images to familiarize themselves with the options available, including:

-	Let Docker manage the storage of your database data [by writing the database files to disk on the host system using its own internal volume management](https://docs.docker.com/storage/volumes/). This is the default and is easy and fairly transparent to the user. The downside is that the files may be hard to locate for tools and applications that run directly on the host system, i.e. outside containers.
-	Create a data directory on the host system (outside the container) and [mount this to a directory visible from inside the container](https://docs.docker.com/storage/bind-mounts/). This places the database files in a known location on the host system, and makes it easy for tools and applications on the host system to access the files. The downside is that the user needs to make sure that the directory exists, and that e.g. directory permissions and other security mechanisms on the host system are set up correctly.

The Docker documentation is a good starting point for understanding the different storage options and variations, and there are multiple blogs and forum postings that discuss and give advice in this area. We will simply show the basic procedure here for the latter option above:

1.	Create a data directory on a suitable volume on your host system, e.g. `/my/own/datadir`.
2.	Start your `%%IMAGE%%` container like this:

	```console
	$ docker run --name some-%%REPO%% -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d %%IMAGE%%:tag
	```

The `-v /my/own/datadir:/var/lib/mysql` part of the command mounts the `/my/own/datadir` directory from the underlying host system as `/var/lib/mysql` inside the container, where MySQL by default will write its data files.

## No connections until MySQL init completes

If there is no database initialized when the container starts, then a default database will be created. While this is the expected behavior, this means that it will not accept incoming connections until such initialization completes. This may cause issues when using automation tools, such as Docker Compose, which start several containers simultaneously.

If the application you're trying to connect to MySQL does not handle MySQL downtime or waiting for MySQL to start gracefully, then a putting a connect-retry loop before the service starts might be necessary. For an example of such an implementation in the official images, see [WordPress](https://github.com/docker-library/wordpress/blob/1b48b4bccd7adb0f7ea1431c7b470a40e186f3da/docker-entrypoint.sh#L195-L235) or [Bonita](https://github.com/docker-library/docs/blob/9660a0cccb87d8db842f33bc0578d769caaf3ba9/bonita/stack.yml#L28-L44).

## Usage against an existing database

If you start your `%%IMAGE%%` container instance with a data directory that already contains a database (specifically, a `mysql` subdirectory), the `$MYSQL_ROOT_PASSWORD` variable should be omitted from the run command line; it will in any case be ignored, and the pre-existing database will not be changed in any way.

## Creating database dumps

Most of the normal tools will work, although their usage might be a little convoluted in some cases to ensure they have access to the `mysqld` server. A simple way to ensure this is to use `docker exec` and run the tool from the same container, similar to the following:

```console
$ docker exec some-%%REPO%% sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql
```

## Restoring data from dump files

For restoring data. You can use `docker exec` command with `-i` flag, similar to the following:

```console
$ docker exec -i some-%%REPO%% sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /some/path/on/your/host/all-databases.sql
```
