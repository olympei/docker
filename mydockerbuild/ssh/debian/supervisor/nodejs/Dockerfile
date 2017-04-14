FROM debian/supervisor
MAINTAINER Thinegan Ratnam <thinegan@thinegan.com>

RUN apt-get update && apt-get install -y nodejs  build-essential
RUN ln -s /usr/bin/nodejs /usr/bin/node

# Define default command
CMD ["/usr/bin/supervisord","-c","/etc/supervisor/supervisord.conf"]
