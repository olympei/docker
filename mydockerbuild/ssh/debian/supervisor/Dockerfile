FROM debian/ssh:dockerfile
MAINTAINER Thinegan Ratnam <thinegan@thinegan.com>

RUN apt-get update && apt-get install -y supervisor
#RUN mkdir /var/log/supervisor

ADD supervisord.conf /etc/supervisor/supervisord.conf

EXPOSE 22
CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]

