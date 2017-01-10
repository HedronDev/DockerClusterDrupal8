FROM ruby
MAINTAINER Kurt Vanderwater <kurt@meridian-ds.com>

RUN gem install mailcatcher --no-rdoc --no-ri

# smtp (25) and ip (80) ports
EXPOSE 25 80

CMD ["mailcatcher", "--smtp-ip=0.0.0.0", "--smtp-port=25", "--http-ip=0.0.0.0", "--http-port=80", "-f"]
