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

ENV DEBIAN_FRONTEND noninteractive

# Gerenare the locales
RUN locale-gen en_US.UTF-8
RUN export LANG=en_US.UTF-8

# Update & upgrade the repositories
RUN apt-get update
RUN apt-get -y --force-yes upgrade

# Install MySQL
RUN apt-get install -y --force-yes mysql-server

# Remove the database dir
RUN rm -rf /var/lib/mysql
RUN mkdir /var/lib/mysql
RUN chown mysql:mysql /var/lib/mysql

# Copy the init files for Mailcacher
ADD 99_setup.sh /etc/my_init.d/99_setup.sh
RUN chmod +x /etc/my_init.d/99_setup.sh

# Make the port 6379 available to outside world
EXPOSE 3306

# Deleting the man pages and documentation
RUN find /usr/share/doc -depth -type f ! -name copyright|xargs rm || true
RUN find /usr/share/doc -empty|xargs rmdir || true
RUN rm -rf /usr/share/man /usr/share/groff /usr/share/info /usr/share/lintian > /usr/share/linda /var/cache/man

# Clean up APT when done.
RUN apt-get clean
RUN apt-get autoclean
RUN apt-get autoremove
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
