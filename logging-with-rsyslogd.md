
## How to log via rsyslogd

Many users prefer legacy rsyslogd's flat file logging over current systemd's more complicated interface. These instructions are the same other than the location of the provided 'util' templates directory. When complete, your WeeWX system will log to flat files in /var/log/weewx.

### For pip installations

```
# copy the rsyslog template file into place and restart the daemon
sudo cp /home/pi/weewx-data/util/rsyslog.d/weewx.conf /etc/rsyslog.d/weewx.conf \
    && sudo systemctl restart rsyslog

# set up logrotate to match
echo "...setting up logrotate..."
sudo cp /home/pi/weewx-data/util/logrotate.d/weewx /etc/logrotate.d/weewx

# restart rsyslogd
sudo systemctl restart rsyslog

# restart weewx
sudo systemctl restart weewx

```

### For packaged installations

```
# copy the rsyslog template file into place and restart the daemon
sudo cp /etc/weewx/util/rsyslog.d/weewx.conf /etc/rsyslog.d/weewx.conf \
    && sudo systemctl restart rsyslog

# set up logrotate to match
echo "...setting up logrotate..."
sudo cp /etc/weewx/util/logrotate.d/weewx /etc/logrotate.d/weewx

# restart rsyslogd
sudo systemctl restart rsyslog

# restart weewx
sudo systemctl restart weewx

```

### About permissions

/var/log/weewx for debian(ish) systems typically prevents world-read, so you might need to add your account to a unix group that has read access.  To do this on a raspberry pi you would `usermod -aG adm my_username_here` for whatever username you are logged into.  On a raspberry pi this frequently is not necessary.  You can check your group membership via the `groups` command.

