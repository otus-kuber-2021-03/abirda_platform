FROM nginx:1.20.0

RUN groupmod -g 1001 nginx && \
    usermod -u 1001 nginx
RUN mkdir /app && chown nginx:nginx /app

USER nginx

COPY --chown=nginx:nginx config/nginx.conf /etc/nginx/nginx.conf
COPY --chown=nginx:nginx config/default.conf /etc/nginx/conf.d/default.conf
RUN mkdir -p /tmp/log/nginx

RUN echo "I'm not an html" > /app/homework.html
EXPOSE 8000
