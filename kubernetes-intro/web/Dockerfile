FROM nginx
COPY app.conf /etc/nginx/conf.d/

RUN  chmod -R 777 /etc/nginx/ \
        /var/log/nginx /var/cache/nginx/  && \
        rm -rf /etc/nginx/conf.d/default.conf && \
        touch /var/run/nginx.pid && \
        chmod 666 /var/run/nginx.pid

USER 1001
# COPY test.html /app
EXPOSE 8000
CMD ["nginx", "-g", "daemon off;"]
