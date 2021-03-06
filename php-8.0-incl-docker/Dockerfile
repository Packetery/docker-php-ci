FROM php:8.0-cli-buster

RUN apt-get update \
	&& apt-get install -y gnupg2 \
	&& apt-get install -y software-properties-common apt-transport-https ca-certificates curl \
	#add docker-ce repo apt key
	&& curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - \
	&& add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
	# Using Ubuntu
    && curl -fsSL https://deb.nodesource.com/setup_14.x | bash -

RUN apt-get update \
	&& apt-get install -y libcurl4-openssl-dev git libxslt-dev mercurial subversion zlib1g-dev graphviz zip libmcrypt-dev libicu-dev g++ libpng-dev libgd-dev libc-client-dev libkrb5-dev nodejs docker-ce \
	&& apt-get install -y libzip-dev

RUN docker-php-ext-install soap \
	&& docker-php-ext-install zip \
	&& docker-php-ext-install xsl \
	&& docker-php-ext-configure gd --with-freetype=/usr/include --with-jpeg=/usr/include \
	&& docker-php-ext-install gd \
	&& docker-php-ext-install gettext \
	&& docker-php-ext-install curl \
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-install pdo_mysql \
	&& docker-php-ext-install intl \
	&& docker-php-ext-install opcache \
	&& docker-php-ext-install bcmath \
	&& docker-php-ext-install sockets \
	&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
	&& docker-php-ext-install imap

#remove libraries for php extension compilation and old versions of nodejs & npm
RUN apt-get -y purge \
		libpng-dev \
		zlib1g-dev \
		libcurl4-openssl-dev \
		libxslt-dev \
		libmcrypt-dev \
		libicu-dev \
		libgd-dev \
		libc-client-dev \
		libkrb5-dev \
	&& apt-get clean

#install npm & gulp & grunt & bower
RUN npm install -g \
	gulp-cli \
	grunt-cli \
	bower

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN echo '{ "allow_root": true, "storage": {"packages": ".bower-cache/"} }' > ~/.bowerrc
RUN echo 'cache=.node-cache' > ~/.npmrc
RUN mkdir ~/.composer
RUN echo '{ "config": { "cache-dir": ".composer-cache" } }' > ~/.composer/composer.json
RUN composer global require --no-progress --no-interaction --no-suggest deployer/deployer deployer/recipes
RUN ln -s ~/.composer/vendor/bin/dep /usr/bin/deployer
RUN curl -O https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh && chmod +x wait-for-it.sh && ln -s ~/wait-for-it.sh /usr/bin/wait-for-it
RUN mkdir ~/.ssh && chmod 700 ~/.ssh
