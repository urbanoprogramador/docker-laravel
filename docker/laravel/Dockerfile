FROM php:7.4-fpm

# Comandos a ejecutar al iniciar el contenedor
RUN apt-get update
RUN apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    autoconf \
    pkg-config \
    libssl-dev \
    libzip-dev \
    libgmp-dev \
    re2c \
    libmhash-dev \
    libmcrypt-dev \
    file \
    zip 

RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/
RUN docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip \
#    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install gmp \
    && docker-php-ext-install gd




# Instalar locale y los idiomas que necesitemos
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y locales locales-all
RUN sed -i 's/# es_CO.UTF-8 UTF-8/es_CO.UTF-8 UTF-8/' /etc/locale.gen \
    && locale-gen

# Instalar drivers Base de datos
RUN apt-get install -y libpq-dev \
    && docker-php-ext-install pdo pdo_pgsql

# Limpiar cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Agregar el usuario laravel que realizará los cambios
RUN groupadd -g 1001 www
RUN useradd -u 1001 -ms /bin/bash -g www www

RUN echo "hola"

#RUN chown -R www:www /var/www
# Usar el usuario wwww
USER www

# Exportar el puerto 9000 de php-fpm
EXPOSE 9000
CMD ["php-fpm"]