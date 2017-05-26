FROM postgres:9.6
ENV PGDATA=/postgres/data
ARG pguser=postgres
RUN mkdir -p $PGDATA ; chown $pguser: -R $PGDATA ; chmod 0700 $PGDATA
USER $pguser
RUN initdb -k -D $PGDATA ; mkdir $PGDATA/pg_log; chown postgres: $PGDATA/pg_log; mkdir $PGDATA/conf_dir;  chown postgres: $PGDATA/conf_dir;echo "include_dir 'conf_dir'" >> $PGDATA/postgresql.conf
COPY *.conf $PGDATA/conf_dir/
RUN echo "host replication postgres pgstandby.my_net  trust" >> $PGDATA/pg_hba.conf
VOLUME ["/postgres"]

CMD pg_ctl -D $PGDATA -o "-c listen_addresses='*' -c logging_collector=on -c log_directory='pg_log'" -l $PGDATA/pg_log/startup.log start  && tail -f /dev/null
