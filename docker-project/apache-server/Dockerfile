FROM httpd:latest

RUN sed -i 's/Listen 80/Listen 8888/' /usr/local/apache2/conf/httpd.conf

EXPOSE 8888
RUN echo "ServerName localhost" >> /usr/local/apache2/conf/httpd.conf
