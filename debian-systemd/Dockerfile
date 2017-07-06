########################################################################
# Dockerfile for a debian container running the systemd init system.
# Heavily borrowed from
# https://github.com/dockerimages/docker-systemd/blob/master/15.10/Dockerfile
#
# You can use it as a base image to build containers that need to run
# more than one process.
# As such, you probably won't run it alone as it does nothing useful but
# starting systemd...
# Anyway, when you run it or any container you build on it, you'll need
# at least the following options for docker run:
#
# --cap-add=SYS_ADMIN -v /sys/fs/cgroup:/sys/fs/cgroup:ro
########################################################################

#FROM debian:jessie
FROM ubuntu:16.04
ENV container docker
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive
RUN cd /lib/systemd/system/sysinit.target.wants/; ls | grep -v systemd-tmpfiles-setup | xargs rm -f $1 \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*; \
rm -f /lib/systemd/system/plymouth*; \
rm -f /lib/systemd/system/systemd-update-utmp*;

# make the console service log us in and let us quit when we type halt
RUN cp /lib/systemd/system/console-getty.service /lib/systemd/system/console-getty.service.orig; \
sed -i 's/--noclear/--noclear --autologin root/' /lib/systemd/system/console-getty.service;

#sed -i 's/^Restart.*//g' /lib/systemd/system/console-getty.service;

#RUN systemctl set-default multi-user.target
RUN systemctl set-default getty.target && \
    echo 'root:root' | chpasswd
ENV init /lib/systemd/systemd
VOLUME [ "/sys/fs/cgroup" ]
ENTRYPOINT ["/lib/systemd/systemd"]
