FROM httpd:alpine

# On dépose notre vhost
COPY proxy.conf /usr/local/apache2/conf/extra/proxy.conf


RUN echo "ServerName localhost" >> /usr/local/apache2/conf/httpd.conf \
 && echo "LoadModule proxy_module modules/mod_proxy.so" >> /usr/local/apache2/conf/httpd.conf \
 && echo "LoadModule proxy_http_module modules/mod_proxy_http.so" >> /usr/local/apache2/conf/httpd.conf \
 && echo "Include conf/extra/proxy.conf" >> /usr/local/apache2/conf/httpd.conf
