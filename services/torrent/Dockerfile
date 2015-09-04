FROM debian

##### Update sources.list and install basic tools and key #####

RUN echo "#dépt paquet propriétaire" > /etc/apt/sources.list.d/non-free.list && echo "deb http://ftp2.fr.debian.org/debian/ jessie main non-free" >> /etc/apt/sources.list.d/non-free.list && echo "deb-src http://ftp2.fr.debian.org/debian/ jessie main non-free" >> /etc/apt/sources.list.d/non-free.list && echo "#dépt dotdeb" > /etc/apt/sources.list.d/dotdeb.list && echo "deb http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list.d/dotdeb.list && echo "deb-src http://packages.dotdeb.org jessie all" >> /etc/apt/sources.list.d/dotdeb.list && echo "#dépt multimedia" > /etc/apt/sources.list.d/debian-multimedia.list && echo "deb ftp://ftp.deb-multimedia.org stable main non-free" >> /etc/apt/sources.list.d/debian-multimedia.list
RUN apt-get update && apt-get install -y --force-yes deb-multimedia-keyring && apt-get install -y vim wget && cd /tmp && wget http://www.dotdeb.org/dotdeb.gpg && apt-key add dotdeb.gpg


##### Upgrade all the system and install the tools for torrent box #####

RUN apt-get update && apt-get dist-upgrade -y && apt-get upgrade -y
RUN apt-get -y install htop supervisor curl automake libcppunit-dev libtool build-essential pkg-config libssl-dev libcurl4-openssl-dev libsigc++-2.0-dev libncurses5-dev nginx vim nano screen subversion apache2-utils curl php5 php5-cli php5-fpm php5-curl php5-geoip git unzip unrar-free rar zip ffmpeg2theora buildtorrent mediainfo

##### Download and install XMLRPC, Libtorrent and rtorrent #####

RUN cd /tmp && svn checkout http://svn.code.sf.net/p/xmlrpc-c/code/stable xmlrpc-c && cd xmlrpc-c/ && ./configure && make && make install
RUN cd /tmp && git clone https://github.com/rakshasa/libtorrent.git && cd libtorrent && git checkout 0.13.4 && ./autogen.sh && ./configure && make && make install
RUN cd /tmp && git clone https://github.com/rakshasa/rtorrent.git && cd rtorrent && git checkout 0.9.4 && ./autogen.sh && ./configure --with-xmlrpc-c && make && make install

##### Download and install RUTorrent #####

RUN ldconfig && cd /var/www && git clone https://github.com/Novik/ruTorrent.git rutorrent
RUN cd /var/www/rutorrent/plugins/ && svn co http://rutorrent-chat.googlecode.com/svn/trunk/ chat && svn co http://rutorrent-logoff.googlecode.com/svn/trunk/ logoff && git clone https://github.com/xombiemp/rutorrentMobile.git mobile && svn checkout http://rutorrent-pausewebui.googlecode.com/svn/trunk/ pausewebui && svn checkout http://svn.rutorrent.org/svn/filemanager/trunk/filemanager && chown --recursive www-data:www-data /var/www/rutorrent

##### Add all the configuration and script files  #####

ADD config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD config/plugin_create_conf.php /var/www/rutorrent/plugins/create/conf.php
ADD config/plugin_filemanager_conf.php /var/www/rutorrent/plugins/filemanager/conf.php
ADD config/fpm_php.ini /etc/php5/fpm/php.ini
ADD config/nginx.conf /etc/nginx/nginx.conf
ADD config/nginx_php.conf /etc/nginx/conf.d/php.conf
ADD config/nginx_cache.conf /etc/nginx/conf.d/cache.conf
ADD config/rutorrent_config.php /var/www/rutorrent/conf/config.php
ADD config/rutorrent_plugins.ini /var/www/rutorrent/conf/plugins.ini
ADD config/init_torrent-rtorrent /etc/init.d/torrent-rtorrent
ADD config/nginx_vhost_rutorrent.conf /etc/nginx/sites-enabled/rutorrent.conf
# ADD config/start.sh /start.sh
# RUN chmod 755 /start.sh

#####  #####
RUN rm -f /etc/nginx/sites-enabled/default

RUN mkdir --parents /home/torrent/downloads /home/torrent/watch /home/torrent/.session
RUN useradd --shell /bin/bash --home /home/torrent torrent
ADD config/home_rtorrent.rc /home/torrent/.rtorrent.rc
RUN chown -R torrent:torrent /home/torrent
RUN chmod 755 /home/torrent

RUN chmod +x /etc/init.d/torrent-rtorrent && update-rc.d torrent-rtorrent defaults

EXPOSE 80

CMD ["/usr/bin/supervisord"]
