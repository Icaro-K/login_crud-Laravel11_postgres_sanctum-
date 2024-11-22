FROM php:8.3-fpm

# Configurar o usuário do sistema
ARG user=Icaro
ARG uid=1000

# Instalar dependências do sistema e PHP
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    libpq-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Instalar extensões PHP para Laravel
RUN docker-php-ext-install pdo_pgsql

# Copiar Composer para o contêiner
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Definir diretório de trabalho
WORKDIR /var/www

# Copiar todos os arquivos do Laravel
COPY . .

# Instalar as dependências do Composer
RUN composer install --no-interaction --prefer-dist --optimize-autoloader

# Criar usuário do sistema
RUN useradd -G www-data,root -u $uid -d /home/$user $user && \
    mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# Install redis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

# Expor a porta PHP-FPM
EXPOSE 9000

# Comando padrão
CMD ["php-fpm"]
