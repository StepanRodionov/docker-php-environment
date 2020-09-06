FROM postgis/postgis:13-master
RUN localedef -i ru_RU -c -f UTF-8 -A /usr/share/locale/locale.alias ru_RU.UTF-8
ENV LANG ru_RU.utf8

RUN mkdir -p /docker-entrypoint-initdb.d
