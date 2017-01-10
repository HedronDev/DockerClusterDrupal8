FROM ubuntu
MAINTAINER Kurt Vanderwater <kurt@meridian-ds.com>

ARG PASSWORD=UJpgrCMinuy83qqm
ARG TERM=ansi
ARG DEBIAN_FRONTEND=noninteractive
ENV NOTVISIBLE "in users profile"
ENV MARIADB_MAJOR 10.1
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 199369E5404BD5FC7D2FE43BCBCB082A1BB943DB \
  && echo "deb http://ftp.osuosl.org/pub/mariadb/repo/$MARIADB_MAJOR/debian jessie main" > /etc/apt/sources.list.d/mariadb.list \
  && { \
    echo 'Package: *'; \
    echo 'Pin: release o=MariaDB'; \
    echo 'Pin-Priority: 999'; \
  } > /etc/apt/preferences.d/mariadb \
  && apt-get update \
  && apt-get install -y \
    openssh-server \
    vim \
    screen \
    tree \
    mariadb-client \
    telnet \
    net-tools \
    git \
    curl \
    gzip \
    unzip \
    php-cli \
    php-mbstring \
    php-curl \
    php-xml \
  && mkdir /var/run/sshd \
  && echo "root:$PASSWORD" | chpasswd \
  && sed -i 's/PermitRootLogin .*$/PermitRootLogin yes/' /etc/ssh/sshd_config \
  && echo "GatewayPorts yes" >> /etc/ssh/sshd_config \
  # SSH login fix. Otherwise user is kicked off after login
  && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd \
  && echo "export VISIBLE=now" >> /etc/profile \
  # Install composer
  && cd /tmp \
  && wget https://getcomposer.org/installer \
  && php installer \
  && mv composer.phar /usr/local/bin/composer \
  && rm /tmp/installer \
  # Install nodejs
  && curl -sL https://deb.nodesource.com/setup_6.x | bash - \
  && apt-get install -y nodejs \
  # Create the user 'lightning'
  && useradd -s /bin/bash -M -b /var/www/html lightning

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
