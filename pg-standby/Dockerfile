FROM postgres:9.6
ENV PGDATA=/postgres/data
ENV primary_server=pgprimary.my_net
ARG pguser=postgres
RUN mkdir -p $PGDATA ; chown $pguser: -R $PGDATA ; chmod 0700 $PGDATA
USER $pguser
VOLUME ["/postgres"]
CMD pg_basebackup -h $primary_server -p 5432 -R -U postgres -D $PGDATA; pg_ctl -D $PGDATA -o "-c listen_addresses='*' -c logging_collector=on -c log_directory='pg_log'" -l $PGDATA/pg_log/startup.log start  && tail -f /dev/null
