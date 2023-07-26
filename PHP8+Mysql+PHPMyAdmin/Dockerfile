# Imagen base de PHP con Apache
FROM php:8.2-rc-apache-bookworm

# Instala Composer
RUN docker-php-ext-install mysqli pdo pdo_mysql \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

# Configura el archivo de virtual host de Apache
COPY apache-config.conf /etc/apache2/sites-available/000-default.conf

# Copia los archivos de tu proyecto de PHP al directorio del servidor web
COPY . /var/www/html

# Establece el directorio de trabajo para el proyecto de React
WORKDIR /var/www/html/app

# Instala Node.js y npm
RUN apt-get update && apt-get install -y curl && curl -sL https://deb.nodesource.com/setup_14.x | bash - && apt-get install -y nodejs

# Copia los archivos del proyecto de React
COPY app/package*.json ./
RUN npm install
COPY app/ ./

# Compila el proyecto de React
RUN npm run build

# Regresa al directorio del servidor web de PHP
WORKDIR /var/www/html

# Expone el puerto en el que se ejecutará la aplicación de PHP
EXPOSE 80

# Define el comando que se ejecutará cuando se inicie el contenedor
CMD ["apache2-foreground"]
