FROM nginx:latest
RUN useradd -r -u 1001 webuser && \
    mkdir /app && \
    chown webuser /app

COPY homework.html /app

RUN sed -i -r 's/listen.+;/listen 8000;/g' /etc/nginx/conf.d/default.conf && \
    sed -i -r 's/root.+;/root \/app;/g' /etc/nginx/conf.d/default.conf && \
    sed -i -r 's/user\s.+;/user webuser;/g' /etc/nginx/nginx.conf
EXPOSE 8000/tcp
