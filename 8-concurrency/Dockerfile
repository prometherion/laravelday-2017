FROM prometherion/php7-nginx:1.1.3

USER root

#
# Application deployment
#
COPY ./app ./

#
# Installing extensions
#
RUN docker-php-ext-install \
		pdo_mysql \
	&& composer require \
		predis/predis