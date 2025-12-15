
## How to integrate weeWX with nginx

Here is one way to integrate weeWX with an nginx webserver.

### weewx 'pip' installation under /home/pi

```
# This example works for debian(ish) operating systems
# and assumes that it is a pip installation under /home/pi 
#
# This will make your weewx dashboard available under
# http://x.x.x.x/weewx/
#

# install nginx
sudo apt-get install -y nginx

# make the weewx output directory
# under nginx's document root
sudo mkdir -p /var/www/html/weewx

# change its user/group/mode so weewx can write to it
sudo chown -R pi:pi /var/www/html/weewx
sudo chmod -R 755 /var/www/html/weewx

# symlink the normal weewx-data public_html tree to point to
# the created subdirectory under nginx's document root
ln -s /var/www/html/weewx /home/pi/weewx-data/public_html

```

### weewx 'dpkg' installation

```
# same as above, with the pathname reflecting a dpkg installation
#
# This will make your weewx dashboard available under
# http://x.x.x.x/weewx/
#

# install nginx
sudo apt-get install -y nginx

# make the weewx output directory
# under nginx's document root
sudo mkdir -p /var/www/html/weewx

# change its user/group/mode so weewx can write to it
sudo chown -R weewx:weewx /var/www/html/weewx
sudo chmod -R 755 /var/www/html/weewx

# a dpkg installation already writes to the correct
# output directory, so no symlink is required

```

