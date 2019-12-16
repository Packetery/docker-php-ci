FROM php:7.1

RUN apt-get update \
	&& apt-get install -y libcurl4-openssl-dev sudo git libxslt-dev mercurial subversion zlib1g-dev graphviz zip libmcrypt-dev libicu-dev g++ libpng-dev libgd-dev libc-client-dev libkrb5-dev rsync wget gnupg2 \
	&& apt-get clean \
	&& docker-php-ext-install soap \
	&& docker-php-ext-install zip \
	&& docker-php-ext-install xsl \
	&& docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install gd \
	&& docker-php-ext-install mcrypt \
	&& docker-php-ext-install mbstring \
	&& docker-php-ext-install gettext \
	&& docker-php-ext-install curl \
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-install pdo_mysql \
	&& docker-php-ext-install json \
	&& docker-php-ext-install intl \
	&& docker-php-ext-install opcache \
	&& docker-php-ext-install bcmath \
	&& docker-php-ext-install sockets \
	&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
	&& docker-php-ext-install imap

RUN echo "deb https://dl.bintray.com/sobolevn/deb git-secret main" | tee -a /etc/apt/sources.list \
	&& wget -qO - https://api.bintray.com/users/sobolevn/keys/gpg/public.key | apt-key add - \
	&& apt-get update && apt-get install -y gawk git-secret

#remove libraries for php extension compilation
RUN apt-get -y purge \
	libc-client-dev \
	libicu-dev \
	libkrb5-dev \
	libpng-dev \
	zlib1g-dev

#install node & npm & gulp & grunt globally
RUN apt-get remove --purge nodejs \
  && apt-get remove --purge npm
RUN curl -sSL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install -y nodejs
RUN npm install -g \
	gulp-cli \
	grunt-cli \
	bower

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN echo '{ "allow_root": true, "storage": {"packages": ".bower-cache/"} }' > ~/.bowerrc
RUN echo 'cache=.node-cache' > ~/.npmrc
RUN echo '{ "config": { "cache-dir": ".composer-cache" } }' > ~/.composer/composer.json
RUN composer global require --no-progress --no-interaction --no-suggest deployer/deployer:"^6" deployer/recipes hirak/prestissimo
RUN ln -s ~/.composer/vendor/bin/dep /usr/bin/deployer
RUN curl -O https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh && chmod +x wait-for-it.sh && ln -s ~/wait-for-it.sh /usr/bin/wait-for-it
RUN mkdir ~/.ssh && chmod 700 ~/.ssh \
	&& touch ~/.ssh/client && chmod 600 ~/.ssh/client \
	&& touch ~/.ssh/client.pub && chmod 644 ~/.ssh/client.pub \
	&& touch ~/.ssh/known_hosts && chmod 644 ~/.ssh/known_hosts \
	&& echo "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config
