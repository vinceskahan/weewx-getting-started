# WeeWX Getting Started Guide

The intent of this document is to provide a gentle introduction to WeeWX and some basics to get you up and running, with pointers to more detailed information elsewhere in the rather large set of weeWX documention available in the various guides, wiki, and frequently asked questions.

## About WeeWX

WeeWX is python software that permits you to:

* connect to your weather station or other sources of sensor data
* save that data into a database
* generate a set of web pages and images to form a 'dashboard' of your data
* and (optionally) publish your weather data to other sites typically on Internet

WeeWX supports many dozens of weather station vendors and models, as well as a variety of external sources of data such as MQTT.  It can also (optionally) upload your weather data to a variety of external sites such as Weather Underground.

WeeWX is is extremely extensible, with many dozens of user-developed additions and integrations you may choose to add, including user-developed dashboards presenting the available data in a variety of ways. It runs on basically any hardware that can run a modern python version 3.7 or later and can be installed in multiple ways, including via pre-built packages installable onto the most common unix/linux platforms.

User support is done via a 'weewx-user' Google group (LINK TODO) that you may subscribe to and view via a web browser, or alternately choose to receive posts via email.

## Some Terminology

WeeWX has some terminology you need to be aware of:

* a 'driver' interfaces with your weather station or other source of data
* a 'skin' presents your data into web pages and/or generated images
* a 'service' adds typically user-developed functionality to weewx core
* an 'extension' packages customizations for easy installation

There are literally dozens of user-developed and user-supported drivers, skins, services, and extensions available to optionally add to your weeWX system.

You might see a few additional terms in the documentation and support forums:

* LOOP data is readings your station emits routinely from its sensors, sometimes every few seconds
* ARCHIVE data is that data summarized by weewx periodically, generally every few minutes

Archive data is what weewx actually saves to its database, generally containing the high/low/average of all the LOOP sensor data during that 'archive_interval', which is by default 300 seconds.

## WeeWX In A Nutshell

For a working weeWX system need you learn how do the following:

* install and configure weeWX itself
* install and configure a webserver to match
* learn how to monitor your system logs
* learn how to change your initial settings

### Installation

WeeWX can be installed in a variety of ways. New users typically use their operating system's native packaging mechanism (apt, yum, rpm, zipper). Other more advanced installation mechanisms (pip, git) are available as well.  For details - see the Quickstart (LINK TODO)

### Configuration

WeeWX is set up via two main configuration files:

* weewx.conf configures weeWX itself
* skin.conf configures a particular skin specifically 

For an initial installation from a pre-built package, the installation will prompt you with a few top-level questions needed to define your system.  For other installation mechanisms you will have to hand-edit weewx.conf to match.  You can also (re)configure your setup using the provided 'weectl' utility to add, delete, tune your system.  Initially simply running the defaults is recommended.

### Generating a Weather Dashboard

The usual weeWX configuration periodically generates a set of web pages and images you can view via a web browser as a weather dashboard, so to speak.  This does not happen in realtime, it happens only periodically based on how you have the weeWX 'archive_interval' set. 

### Integrating WeeWX With Your Webserver

WeeWX does not come with a web server of its own. It is expected that the user will install and configure a webserver of their choice, and set that webserver up appropriately so weewx can save its generated web pages and images into a directory the webserver can read.  This is frequently the source of some initial issues, which can be hard to work through for users not familiar with how webservers or unix permissions work.

In short - weeWX runs as a unprivileged user ('weewx' for a packaged installation) that needs to be able to write to a directory owned by the webserver process ('www-data' for some webserver packages).  Initially it is helpful to set debug=1 in weewx.conf to make the debugging information more verbose to help you get things integrated successfully.

There are so many webserver variants that it is impossible to list the differences here, but typically users install 'nginx' or 'apache' or whatever is their preferred package.  WeeWX doesn't require any particular webserver package.  It only needs to be able to write to the webserver's HTML document tree.

Some possible ways to do this are available here (LINK TODO)

For nginx on a debian(ish) system, one way to do so is [HERE](integrate-weewx-with-nginx.md).   For more possibilities see the (https://www.weewx.com/docs/5.2/usersguide/webserver/)[User's Guide].

### Viewing Log Files

For new users, finding and viewing log files is typically difficult to learn.  By default weeWX logs via your operating system's default logging mechanism, which currently tends to be 'systemd'.  Systemd can be complicated for many users and its interface can be painful.  This is the operating system's choice, unfortunately.

Many users choose to customize their operating system to add the legacy 'rsyslogd' type of logging which writes simple flat files, and configure the legacy 'logrotate' tool to periodically rotate the logs.  Template files for both rsyslogd and logrotate are provided with weeWX and it's only a few one-time steps to configure logging to work in a legacy rsyslogd type of mode.

For details - see (LINK TODO)

## For More Information

Please see the following:

* the weewx-user Google Group
* wiki
* faq
* guides....list'em out
integrate-weewx-with-nginx.md
