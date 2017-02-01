# this is based on ../7.7/Dockerfile
FROM php:apache
MAINTAINER Martin Scharm <https://binfalse.de/contact/>

# Install requirements
RUN apt-get update &&\
    apt-get install -y -q --no-install-recommends \
        wget \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        zlib1g-dev \
        locales \
 && echo en_US.UTF-8 UTF-8 >> /etc/locale.gen \
 && echo en_GB.UTF-8 UTF-8 >> /etc/locale.gen \
 && /usr/sbin/locale-gen \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*

# Install typo3
RUN cd /var/www/html && \
    wget -O - https://get.typo3.org/7 | tar -xzf - && \
    ln -s typo3_src-* typo3_src && \
    ln -s typo3_src/index.php && \
    ln -s typo3_src/typo3 && \
    ln -s typo3_src/_.htaccess .htaccess && \
    mkdir typo3temp && \
    mkdir typo3conf && \
    mkdir fileadmin && \
    mkdir uploads && \
    chown -R www-data: .

# Configure PHP
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) mysqli soap gd zip
RUN echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmax_input_vars = 1500\nupload_max_filesize = 32M\npost_max_size = 32M' > /usr/local/etc/php/conf.d/typo3.ini

# Configure Apache as needed
RUN a2enmod rewrite

# Configure volumes
VOLUME /var/www/html/fileadmin
VOLUME /var/www/html/typo3conf
VOLUME /var/www/html/typo3temp
VOLUME /var/www/html/uploads
