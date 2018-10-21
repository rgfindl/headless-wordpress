FROM wordpress:latest

# Install unzip
RUN apt-get update; \
    apt-get install -y --no-install-recommends unzip

# Install WP plugins
RUN curl -L https://downloads.wordpress.org/plugin/amazon-s3-and-cloudfront.2.0.zip -o /tmp/amazon-s3-and-cloudfront.2.0.zip
RUN unzip /tmp/amazon-s3-and-cloudfront.2.0.zip -d /usr/src/wordpress/wp-content/plugins
RUN rm /tmp/amazon-s3-and-cloudfront.2.0.zip

RUN curl -L https://downloads.wordpress.org/plugin/advanced-custom-fields.5.7.7.zip -o /tmp/advanced-custom-fields.5.7.7.zip
RUN unzip /tmp/advanced-custom-fields.5.7.7.zip -d /usr/src/wordpress/wp-content/plugins
RUN rm /tmp/advanced-custom-fields.5.7.7.zip

RUN curl -L https://downloads.wordpress.org/plugin/custom-post-type-ui.1.5.8.zip -o /tmp/custom-post-type-ui.1.5.8.zip
RUN unzip /tmp/custom-post-type-ui.1.5.8.zip -d /usr/src/wordpress/wp-content/plugins
RUN rm /tmp/custom-post-type-ui.1.5.8.zip