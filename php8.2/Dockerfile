FROM php:8.2-apache

ENV ID_USER=1000
ENV ID_GROUP=1000

# Nommage du serveur apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Installation de composant
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
		locales \
		apt-utils \
		git \
		libicu-dev \
		g++ \
		libpng-dev \
		libxml2-dev \
		libzip-dev \
		libonig-dev \
		libxslt-dev \
		vim \
		unzip \
		;

# Paramétrage de l'encodage
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

# Installation conposer
RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
   mv composer.phar /usr/local/bin/composer

# Installation Symfony Cli
RUN curl -sS https://get.symfony.com/cli/installer | bash && \
   mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

# Installation extension PHP
RUN docker-php-ext-configure \
	intl
RUN docker-php-ext-install \
	pdo \
	pdo_mysql \
	gd opcache \
	intl \
	zip \
	calendar \
	dom \
	mbstring \
	zip \
	gd \
	xsl

# Ne fonctionne pas sur linux
#RUN pecl install apcu && docker-php-ext-enable apcu

# Installation de node & yarn
#RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
RUN apt-get install -y nodejs
RUN corepack enable


RUN addgroup --gid ${ID_GROUP} groupdev
RUN adduser -uid ${ID_USER} --ingroup groupdev --home /home/develop develop

USER develop

COPY .bashrc /home/develop

# Définition du répertoire de travail par défaut
WORKDIR /var/www