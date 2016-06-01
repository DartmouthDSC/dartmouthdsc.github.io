---
title: Installation Guide
---
{% include toc %}

_Note: This guide is specific to Dartmouth's implementation._


This page describes the installation process for the servers/applications needed for most of the applications we run. The following were installed on our qa server:

## Apache 2.2
*Note: This already came installed on our VM.*

Changed `chkconfig` to be `345 99 01` in `/etc/init.d/httpd` because Apache should be the last thing to start up and the first thing to shut down.

After changing chkconfig run:

``` bash
sudo chkconfig --del httpd
sudo chkconfig --add httpd
```

## Apache Modules Added

### Passenger
Need to install from tarball because RPM is not up to date on the latest packages.
Instructions here:
<https://www.phusionpassenger.com/library/install/apache/install/oss/tarball/>

### mod_ssl
Ran `yum install mod_ssl`

## Redis 2.8.19
Followed the Redis installations instructions here: <http://redis.io/topics/quickstart>

Because we are running a Red Hat VM, the service script needs some special comments at the beginning of the file in order to set the default run levels.

These slight variations from the quick start guide were described here:
http://vincentvandaal.nl/271/redis-quick-install-on-redhat-based-distro/

Change `chkconfig` to be `345 95 05`. After changing `chkconfig`, delete the previously set run levels and set the new levels. Run:

```bash
sudo chkconfig --del redis_6379
sudo chkconfig --add redis_6379
```
**Warning!** Unexpected results can happen if the previous run levels are not deleted first. Make sure to delete the previous run levels.
{: .notice--danger}

Redis is at port 6379.

## Jetty 9.x.x
Followed these instructions for Jetty installation:
<http://www.eclipse.org/jetty/documentation/current/startup-unix-service.html>

Not all the modules needed for Fedora are listed in the startup-unix documentation. For more information about specific modules: <http://eclipse.org/jetty/documentation/current/startup-modules.html>

Fedora requires the jsp module, so be sure to add it to the list of modules needed to the start.jar command. The start.jar command should append jsp as such:

`sudo java -jar /opt/jetty/jetty-distribution-9.2.10.v20150310/start.jar --add-to-start=deploy,http,logging,jsp`

The Jetty service script provides default run levels, these should probably be updated to be more accurate and set to our specific needs. Currently, they are set to `345 90 10`. After run levels are updated, run this command to set the default run levels:

```bash
sudo chkconfig --del jetty
sudo chkconfig --add jetty
```
Jetty is at port 8080.

## Fedora 4.x.x
Followed these instructions for Fedora installation:
<https://wiki.duraspace.org/display/FEDORA40/Deploying+Fedora+4+Complete+Guide>

__Note that the default webapps directory changed in Jetty 9.__

#### Adding WAR file and updating fedora.xml
The Fedora WAR file can simply be dropped in `/opt/fedora`. The `fedora.xml` file in `/usr/local/dac-conf/jetty-webapps` should be updated to point at the new WAR file. Then, run `sudo make` in that directory to move the edited xml file into `/opt/web/mybase/webapps/fedora.xml`

More information about using an xml file to deploy a webapp:  <http://eclipse.org/jetty/documentation/current/configuring-specific-webapp-deployment.html>


Example of `fedora.xml` file file:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE Configure PUBLIC "-//Jetty//Configure//EN" "http://www.eclipse.org/jetty/configure_9_0.dtd">
	 <Configure class="org.eclipse.jetty.webapp.WebAppContext">
		 <Set name="contextPath">/fedora</Set>
	     <Set name="war">/opt/fedora/fcrepo-webapp-4.2.0.war</Set>
	 </Configure>
```

#### Changes to `/etc/defaults/jetty`
- Changed java.io.tmpdir location to match the same tmpdir as Jetty 9.
- Fedora 4.2.0 requires Java 1.8, therefore the version of java Jetty is using needs to point to Java 1.8.
```
JAVA=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.51-1.b16.el6_7.x86_64/jre/bin/java
JAVA_OPTIONS="${JAVA_OPTIONS} -Dfcrepo.home=/mnt/fedora-data -Djava.io.tmpdir=/opt/jetty/temp"
```

Fedora is a webapp deployed from within Jetty, so its also at port 8080.

Fedora path is `localhost:8080/fedora/rest`.

## Solr 5.x.x
Installing Solr 5.x.x in production: <https://cwiki.apache.org/confluence/display/solr/Taking+Solr+to+Production>

Run levels are set up in the Solr service script using LSB-style, currently we don’t have the modules installed to read this style, so the following lines have to be added to the beginning of the script in order to set up the right run levels and order:

```bash
###############
# Runlevel configuration needs to be set in Red Hat's style not LSB-style.
# chkconfig: 345 95 05
# description: Controls Apache Solr as a Service
###############
```

After those lines have been added run the following commands to update the run levels:

```bash
sudo chkconfig --del jetty
sudo chkconfig --add jetty
```

Solr is at port 8983.
