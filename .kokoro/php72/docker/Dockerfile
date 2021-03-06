FROM gcr.io/google_appengine/php72

RUN apt-get update && apt-get install -y \
    python-ipaddr \
    autoconf \
    build-essential \
    zlib1g-dev \
    jq

RUN git clone https://github.com/GoogleCloudPlatform/php-tools.git ${HOME}/php-tools \
    && ${HOME}/php-tools/scripts/install_test_deps.sh

# install / enable PHP extensions
RUN pecl install grpc \
    && echo "extension=grpc.so" >> /opt/php72/lib/conf.d/ext-grpc.ini \
    && echo "extension=bcmath.so" >> /opt/php72/lib/conf.d/ext-bcmath.ini

# Install composer globally
RUN composer global require phpunit/phpunit:^5.0

# Install Google Cloud SDK
RUN curl https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz \
        -o ${HOME}/google-cloud-sdk.tar.gz \
    && tar xzf ${HOME}/google-cloud-sdk.tar.gz -C $HOME \
    && ${HOME}/google-cloud-sdk/install.sh \
        --usage-reporting false \
        --path-update false \
        --command-completion false

ENV PATH="$PATH:/opt/composer/vendor/bin:/root/google-cloud-sdk/bin"

ENTRYPOINT /bin/bash
