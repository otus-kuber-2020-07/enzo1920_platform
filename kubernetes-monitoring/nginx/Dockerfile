FROM nginx:1.19.1
RUN mkdir /app
COPY default.conf status.conf  /etc/nginx/conf.d/
COPY homework.html /app
#RUN chown -R 1001 /var/cache/nginx && \
#    chown -R 1001 /app && \
#    touch /var/run/nginx.pid && \
#    chown -R 1001 /var/run/nginx.pid
#USER 1001
EXPOSE 8080