# WeeWX Getting Started Guide

The intent of this document is to provide a gentle introduction to WeeWX and some basics to get you up and running, with pointers to more detailed information elsewhere in the rather large set of WeeWX documention available in the various guides, wiki, and frequently asked questions.

This document links to weewx-5.2 authoritative documents.

To see documentation for other versions, those are available [HERE](https://www.weewx.com/docs.html)

## About WeeWX

WeeWX is python software that permits you to:

* connect to your weather station or other sources of sensor data
* save that data into a database
* generate a set of web pages and images to form a 'dashboard' of your data
* and (optionally) publish your weather data to other sites typically on Internet

WeeWX supports many dozens of weather station vendors and models, as well as a variety of external sources of data such as MQTT.  It can also (optionally) upload your weather data to a variety of external sites such as Weather Underground.

WeeWX is extremely extensible, with many dozens of user-developed additions and integrations you may choose to add, including user-developed dashboards presenting the available data in a variety of ways. It can run on basically any hardware that can run a modern python version 3.7 or later and may be installed in multiple ways, including via pre-built packages installable onto the most common unix/linux platforms.

User support is done via the weewx-user [Google Group](https://groups.google.com/g/weewx-user) that you may subscribe to and view via a web browser, or you may alternately choose to receive posts via email.

## Some Terminology

WeeWX has some terminology you need to be aware of:

* a 'driver' interfaces with your weather station or other source of data
* a 'skin' presents your data into web pages and/or generated images
* a 'service' adds typically user-developed functionality to WeeWX core
* an 'extension' packages customizations for easy installation
* an 'uploader' sends data to an external site or service

There are literally dozens of each available to optionally add to your WeeWX system.  Consult the [Wiki](https://github.com/weewx/weewx/wiki) for a list.

You might see a few additional terms in the documentation and support forums:

* LOOP data is readings your station emits routinely from its sensors, sometimes every few seconds
* ARCHIVE data is that data summarized by WeeWX periodically, generally every few minutes

Archive data is what WeeWX actually saves to its database, generally containing the high/low/average of all the LOOP sensor data during that `archive_interval`, which is by default 300 seconds.

> [!CAUTION]
> Setting your archive_interval too low (fast) can interfere with weewx operation on slow systems.

## WeeWX In A Nutshell

For a working WeeWX system need you learn how do the following:

* install and configure WeeWX itself
* install and configure a webserver to match
* learn how to monitor your system logs
* learn how to change your initial settings
* learn how to report a problem

### Installation

WeeWX can be installed in a variety of ways. New users typically use their operating system's native packaging mechanism (apt, yum, rpm, zipper). Other more advanced installation mechanisms (pip, git) are available as well.

For details and procedures - start with the [WeeWX Quick start](https://www.weewx.com/docs/5.2/#installation)

### Configuration

WeeWX is set up via two main configuration files:

* `weewx.conf` configures WeeWX itself
* `skin.conf` configures each particular skin specifically 

For an initial installation from a pre-built package, the installation will prompt you with a few top-level questions needed to define your system.  For other installation mechanisms you will have to hand-edit weewx.conf to match. The provided `weectl` utility is used to add to, delete from, or tune your system, although some hand-editing of weewx.conf might be needed as well..

Initially simply running the defaults is recommended.

### Generating a Weather Dashboard

The usual WeeWX configuration periodically generates a set of web pages and images you can view via a web browser as a weather dashboard, so to speak.

This does not happen in realtime, it happens only periodically based on how you have the 'archive_interval' configuration item set in weewx.conf.

### Integrating WeeWX With Your Webserver

WeeWX does not come with a web server of its own. It is expected that the user will install and configure a webserver of their choice so that WeeWX can save its generated web pages and images into a directory the webserver can read.  This is frequently the source of some initial issues, which can be hard to work through for users not familiar with how webservers or unix permissions work.

In short - WeeWX runs as a unprivileged user ('weewx' for a packaged installation) that needs to be able to write to a directory owned by the webserver process (which may differ depending on which webserver you install).

WeeWX does not require any particular webserver package.  It only needs to be able to write to the webserver's HTML document tree.

  There are so many webserver variants that it is impossible to list the differences here, but typically users install 'nginx' or 'apache' or whatever is their preferred package.

* For nginx on a debian(ish) system, one way to do so is [HERE](integrate-weewx-with-nginx.md)
* For more possibilities see the [WeeWX User's Guide](https://www.weewx.com/docs/5.2/usersguide/webserver/).

> [!TIP]
>Initially it is helpful to set debug=1 in weewx.conf to make the debugging information more verbose to help you get things integrated successfully.

### Viewing Log Files

For new users, finding and viewing log files is typically difficult to learn.

By default WeeWX logs via your operating system's default logging mechanism, which currently tends to be 'systemd'. This is the operating system's choice, unfortunately.

Systemd can be complicated for many users and its interface can be painful.  For details on how to use systemd's 'systemctl' command, consult your operating system's manual pages, do a Google search, or see the WeeWX documentation's discussion regarding logging [HERE](https://www.weewx.com/docs/5.2/usersguide/monitoring/).

> [!TIP]
> Alternately, many users choose to customize their operating system to add the legacy 'rsyslogd' type of logging which writes simple flat files, which requires also installing and configuring the legacy 'logrotate' tool to periodically rotate the logs.
>
> Template files for both rsyslogd and logrotate are provided with WeeWX and it's only a few one-time steps to configure logging to work in a legacy rsyslogd type of mode.  For details - see [HERE](logging-with-rsyslogd.md).

### How To Change Your Initial Settings

WeeWX provides the `weectl` utility for configuring much of the system settings, as well as other operations.  For details see the documentation [HERE](https://www.weewx.com/docs/5.2/utilities/weectl-about/).

In other cases, you might need to manually edit weewx.conf or a skin.conf file, or even a html .tmpl template file within a skin.  If you edit weewx.conf, you will need to restart weewx to make the changes take effect.  Changes to skin.conf or a skin's .tmpl HTML templates take effect when weewx runs its periodic reports when the next archive_interval rolls around.

### How To Change Your Weather Dashboard

By default, the Seasons 'skin' is enabled when your initial installation is complete, with many other possible dashboards provided as skins by WeeWX. To enable/disable a particular skin, simply set 'enable=true' or 'enable=false' in its section in weewx.conf, then restart WeeWX.

You may optionally install other skins via the `weectl extension` installer.  There is a long list of some available user-developed skins in the WeeWX Wiki.

You can also create your own custom skin from scratch, which is a much more advanced topic.  See the [Customization Guide](https://www.weewx.com/docs/5.2/custom/introduction/) for details.

### Uploading Your Data To Internet Sites

Many users choose to upload their data periodically to commonly used Internet sites such as Weather Underground.  This is enabled by hand-editing weewx.conf and setting the required parameters in that site's section therein, setting enable=true, and restarting weewx.

Uploaders for a half-dozen well-known sites are included in WeeWX core, with several dozen more that can be added. See the Wiki for many user-provided uploaders and links to each item's installation and configuration steps.

### How To Report A Problem

When you need to report a problem or ask a HOWTO type question, the users in weewx-users typically need more information about your setup, what you did, and what you are seeing.  It can take quite a few back+forth questions and answers to get enough information to try to help.

See [HERE](how-to-report-a-problem.md) as one way to do a good problem report.

## For More Information

Please see the following:

* the weewx-user [Google Group](https://groups.google.com/g/weewx-user) 
* the WeeWX [Wiki](https://github.com/weewx/weewx/wiki)
* the WeeWX [Frequently Asked Questions](https://github.com/weewx/weewx/wiki/WeeWX-Frequently-Asked-Questions) aka FAQ
* the very detailed [WeeWX Documentation](https://www.weewx.com/docs) 

