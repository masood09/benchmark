# Use phusion/baseimage as base image.
FROM phusion/baseimage:0.9.17

MAINTAINER Masood Ahmed masood.ahmed09@gmail.com

# Set correct environment variables.
ENV HOME /root

# Set noninteractive mode for apt-get
ENV DEBIAN_FRONTEND noninteractive

# Regenerate SSH host keys. baseimage-docker does not contain any, so we
# have to do that yourself.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Gerenare the locales
RUN locale-gen en_US.UTF-8
RUN export LANG=en_US.UTF-8

# Add the required repositories
RUN apt-get install apt-transport-https
RUN add-apt-repository -y ppa:ondrej/php5-5.6
RUN apt-key adv --keyserver keys.gnupg.net --recv-keys 4F4EA0AAE5267A6C

# Update & upgrade the repositories
RUN apt-get update
RUN apt-get -y --force-yes upgrade

# Install PHP5 (5.6.11) and apache2
RUN apt-get install -y --force-yes wget apache2 php5-cli php5-dev php5-mysqlnd php5-curl php5-gd php5-gmp php5-mcrypt php5-xmlrpc libwebp-dev php5-intl libapache2-mod-php5

# Enable required apache2 modules
RUN a2enmod rewrite
RUN a2enmod expires
RUN a2enmod headers

# Install n98-magerun
RUN wget http://files.magerun.net/n98-magerun-latest.phar -O /usr/local/bin/magerun
RUN chmod +x /usr/local/bin/magerun

# Copy the init files for webserver
ADD 99_start.sh /etc/my_init.d/99_start.sh
RUN chmod +x /etc/my_init.d/99_start.sh

# Copy the magento conf files
RUN rm -rf /etc/apache2/sites-enabled/000-default.conf
ADD magento.conf /etc/apache2/sites-enabled/magento.conf

# Make the port 80 and 443 available to outside world
EXPOSE 80
EXPOSE 443

# Clean doc and man files.
RUN find /usr/share/doc -depth -type f ! -name copyright|xargs rm || true
RUN find /usr/share/doc -empty|xargs rmdir || true
RUN rm -rf /usr/share/man /usr/share/groff /usr/share/info /usr/share/lintian > /usr/share/linda /var/cache/man

# Clean up APT when done.
RUN apt-get clean
RUN apt-get autoclean
RUN apt-get autoremove
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
