FROM golang:1.11 as operator

RUN go get -v github.com/kubedb/postgres/cmd/pg-operator

FROM sophox/postgis

COPY --from=operator /go/bin/pg-operator /usr/bin
RUN apt-get update && apt-get install -y curl tar unzip \
 && curl -L -O https://github.com/wal-g/wal-g/releases/download/v0.1.15/wal-g.linux-amd64.tar.gz \
 && tar -xvf wal-g.linux-amd64.tar.gz \
 && cp wal-g /usr/bin/wal-g \
 && chmod +x /usr/bin/wal-g \
 && rm wal-g.linux-amd64.tar.gz

RUN apt-get install -y make gcc

RUN curl -L -O https://github.com/ncopa/su-exec/archive/master.zip \
 && unzip master.zip \
 && cd su-exec-master \
 && make \
 && cp su-exec /usr/bin \
 && chmod +x /usr/bin/su-exec


RUN apt-get remove -y unzip curl make gcc && apt-get autoremove -y \
 && rm -rf /var/cache/apt/* && rm -rf /var/lib/apt/*

ENV PV /var/pv
ENV PGDATA $PV/data
ENV PGWAL $PGDATA/pg_wal
ENV INITDB /var/initdb
ENV WALG_D /etc/wal-g.d/env

COPY scripts /scripts

VOLUME ["$PV"]

ENV STANDBY warm
ENV RESTORE false
ENV BACKUP_NAME LATEST
ENV PITR false
ENV ARCHIVE_S3_PREFIX ""
ENV RESTORE_S3_PREFIX ""

ENV ARCHIVE_GS_PREFIX ""
ENV RESTORE_GS_PREFIX ""

ENTRYPOINT ["pg-operator"]
CMD ["leader_election"]

EXPOSE 5432

