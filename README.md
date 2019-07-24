This is just like the [official docker WordPress images](https://github.com/docker-library/wordpress),
but without WordPress itself.  It is therefore suitable for installing WordPress using composer.

There is currently only one Dockerfile, for php:7.3-fpm-alpine.  Follow
[this issue](https://github.com/docker-library/wordpress/issues/418)
for something better.  I hope.

What's different?  Precisely this:
```
$ diff ~/Downloads/docker-wordpress/php7.3/fpm-alpine/Dockerfile Dockerfile
3,9d2
< # docker-entrypoint.sh dependencies
< RUN apk add --no-cache \
< # in theory, docker-entrypoint.sh is POSIX-compliant, but priority is a working, consistent image
< 		bash \
< # BusyBox sed is not sufficient for some of our sed expressions
< 		sed
< 
66,79d58
< ENV WORDPRESS_VERSION 5.2.2
< ENV WORDPRESS_SHA1 3605bcbe9ea48d714efa59b0eb2d251657e7d5b0
< 
< RUN set -ex; \
< 	curl -o wordpress.tar.gz -fSL "https://wordpress.org/wordpress-${WORDPRESS_VERSION}.tar.gz"; \
< 	echo "$WORDPRESS_SHA1 *wordpress.tar.gz" | sha1sum -c -; \
< # upstream tarballs include ./wordpress/ so this gives us /usr/src/wordpress
< 	tar -xzf wordpress.tar.gz -C /usr/src/; \
< 	rm wordpress.tar.gz; \
< 	chown -R www-data:www-data /usr/src/wordpress
< 
< COPY docker-entrypoint.sh /usr/local/bin/
< 
< ENTRYPOINT ["docker-entrypoint.sh"]
```
