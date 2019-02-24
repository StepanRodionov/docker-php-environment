FROM nginx:alpine

RUN mkdir /var/cache/nginx/resized && rm -rf /etc/nginx/conf.d/default.conf


COPY ./nginx.conf /etc/nginx/
COPY ./conf.d/ /etc/nginx/conf.d/

EXPOSE 9001