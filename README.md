[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/cops
[![](https://images.microbadger.com/badges/version/lsioarmhf/cops.svg)](https://microbadger.com/images/lsioarmhf/cops "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/cops.svg)](http://microbadger.com/images/lsioarmhf/cops "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/cops.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/cops.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io-armhf/lsioarmhf-cops)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io-armhf/job/lsioarmhf-cops/)
[hub]: https://hub.docker.com/r/lsioarmhf/cops/

COPS, by Sébastien Lucas, stands for Calibre OPDS (and HTML) Php Server.

COPS links to your Calibre library database and allows downloading and emailing of books directly from a web browser and provides a OPDS feed to connect to your devices.

Changes in your Calibre library are reflected immediately in your COPS pages.

See : [COPS's home](http://blog.slucas.fr/en/oss/calibre-opds-php-server) for more details.

Don't forget to check the [Wiki](https://github.com/seblucas/cops/wiki).

## Why? (taken from the author's site)

In my opinion Calibre is a marvelous tool but is too big and has too much
dependencies to be used for its content server.

That's the main reason why I coded this OPDS server. I needed a simple
tool to be installed on a small server (Seagate Dockstar in my case).

I initially thought of Calibre2OPDS but as it generate static file no
search was possible.

Later I added an simple HTML catalog that should be usable on my Kobo.

So COPS's main advantages are :
 * No need for many dependencies.
 * No need for a lot of CPU or RAM.
 * Not much code.
 * Search is available.
 * With Dropbox / owncloud it's very easy to have an up to date OPDS server.
 * It was fun to code.

If you want to use the OPDS feed don't forget to specify feed.php at the end of your URL.

[![cops](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/cops-icon.png)][copsurl]
[copsurl]: http://blog.slucas.fr/en/oss/calibre-opds-php-server

## Usage

```
docker create \
	--name=cops \
	-v <path to data>:/config \
	-v <path to data>:/books \
	-e PGID=<gid> -e PUID=<uid>  \
	-e TZ=<timezone> \
	-p 80:80 \
	lsioarmhf/cops
```

**Parameters**

* `-p 80` - the port(s)
* `-v /config` - COPS Application Data
* `-v /books` - Calibre metadata.db location
* `-e PGID` for for GroupID - see below for explanation
* `-e PUID` for for UserID - see below for explanation
* `-e TZ` for timezone information, eg Europe/London

It is based on alpine-linux with S6 overlay, for shell access whilst the container is running do `docker exec -it cops /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 
`IMPORTANT... THIS IS THE ARMHF VERSION`

Unlike other implementations of COPS in a docker container,  the linuxserver version gives you access to `config_local.php` in `/config` to customise your install to suit your needs, including details of your email account etc to enable emailing of books, it also includes the dependencies required to directly view epub books in your browser.

## Info

* To monitor the logs of the container in realtime `docker logs -f cops`.

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' cops`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/cops`

## Versions

+ **14-10-16:** Add version layer information.
+ **28.09.16:** Add php5-zlib.
+ **11.09.16:** Add layer badges to README.
+ **29.08.16:** Add php5-opcache.
+ **28.08.16:** Add badges to README.
+ **12.08.16:** Release
