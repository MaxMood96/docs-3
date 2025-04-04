## `%%IMAGE%%:<version>-fpm`

This variant contains [PHP's FastCGI Process Manager (FPM)](https://www.php.net/fpm), which is the recommended FastCGI implementation for PHP.

In order to use this image variant, some kind of reverse proxy (such as NGINX, Apache, or other tool which speaks the FastCGI protocol) will be required.

Some potentially helpful resources:

-	[FPM's Official Configuration Reference](https://www.php.net/manual/en/install.fpm.configuration.php)
-	[Simplified example by @md5](https://gist.github.com/md5/d9206eacb5a0ff5d6be0)
-	[Very detailed article by Pascal Landau](https://www.pascallandau.com/blog/php-php-fpm-and-nginx-on-docker-in-windows-10/)
-	[Stack Overflow discussion](https://stackoverflow.com/q/29905953/433558)
-	[Apache httpd Wiki example](https://wiki.apache.org/httpd/PHPFPMWordpress)

**WARNING:** the FastCGI protocol is inherently trusting, and thus *extremely* insecure to expose outside of a private container network -- unless you know *exactly* what you are doing (and are willing to accept the extreme risk), do not use Docker's `--publish` (`-p`) flag with this image variant.
