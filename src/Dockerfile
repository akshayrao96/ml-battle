########################################################
# Dockerfile for project: classification-battle
########################################################
# This dockerfile was created based on:
# - https://hub.docker.com/r/renangbarreto/apache-php5-phantomjs-docker/
# - https://hub.docker.com/r/renangbarreto/deeplearning-docker/
########################################################

FROM php:8.2-apache

# Install required system dependencies
RUN apt-get update && apt-get install -y \
	apache2 \
	python3 python3-pip python3-venv \
	tesseract-ocr tesseract-ocr-eng \
	software-properties-common wget build-essential \
	libpq-dev libssl-dev openssl libffi-dev zlib1g-dev \
	libonig-dev libxml2-dev \
	&& apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install mbstring xml

# Create and activate a virtual environment
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Install Python packages in the virtual environment
RUN /opt/venv/bin/pip install --upgrade pip && \
	/opt/venv/bin/pip install scikit-learn==1.6.1 joblib==1.4.2 numpy==2.2.2

# Enable shell execution in PHP
RUN echo "disable_functions =" > /usr/local/etc/php/conf.d/custom.ini

# Set permissions for the application
RUN chown -R www-data:www-data /var/www && \
	chmod -R 755 /var/www

# Copy custom Apache configuration
COPY apache.conf /etc/apache2/sites-available/000-default.conf

RUN a2ensite 000-default && a2enmod rewrite

# Start Apache in foreground mode
CMD ["apache2-foreground"]
