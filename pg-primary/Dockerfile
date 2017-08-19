FROM postgres:9.6
ENV PGDATA=/postgres/data
ENV POSTGRES_PASSWORD=password
ARG pguser=postgres
RUN mkdir -p $PGDATA && chown $pguser: -R $PGDATA && chmod 0700 $PGDATA && echo $POSTGRES_PASSWORD >> /passwd_file && chown $pguser: /passwd_file
USER $pguser
RUN initdb -k --pwfile=/passwd_file -A md5 -D $PGDATA ; mkdir $PGDATA/pg_log && chown postgres: $PGDATA/pg_log &&  mkdir $PGDATA/conf_dir &&  chown postgres: $PGDATA/conf_dir && echo "include_dir 'conf_dir'" >> $PGDATA/postgresql.conf
COPY *.conf $PGDATA/conf_dir/
COPY pg_hba.conf.additions /tmp
RUN cat /tmp/pg_hba.conf.additions  >> $PGDATA/pg_hba.conf
VOLUME ["/postgres"]

CMD pg_ctl -D $PGDATA -o " -c logging_collector=on -c log_directory='pg_log'" -l $PGDATA/pg_log/startup.log start  && tail -f /dev/null
