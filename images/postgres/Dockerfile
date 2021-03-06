ARG FROM
FROM ${FROM} as build

ARG URL
ARG REF
RUN git clone ${URL} ~/src \
    && cd ~/src \
    && git checkout ${REF} \
    && autoconf \
    && ./configure --prefix=${HOME}/out \
    && make install

RUN mkdir -p \
        ~/rootfs/etc \
        ~/rootfs/lib/x86_64-linux-gnu \
        ~/rootfs/lib64 \
        ~/rootfs/data \
        ~/rootfs/config \
        ~/rootfs/bin \
        ~/rootfs/tmp \
        ~/rootfs/usr/lib/locale \
    && echo "nobody:x:100" > ~/rootfs/etc/group \
    && echo "nobody:x:100:100:nobody:/:/usr/bin/nologin" > ~/rootfs/etc/passwd \
    && echo "en_US.UTF-8 UTF-8" > ~/rootfs/etc/locale.gen \
    && cp -a /usr/lib/locale/. ~/rootfs/usr/lib/locale/ \
    && cp /lib/x86_64-linux-gnu/libreadline.so.7 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libreadline.so.7 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libtinfo.so.6 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libpthread.so.0 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/librt.so.1 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libdl.so.2 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libm.so.6 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libc.so.6 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib/x86_64-linux-gnu/libnss_files.so.2 ~/rootfs/lib/x86_64-linux-gnu \
    && cp /lib64/ld-linux-x86-64.so.2 ~/rootfs/lib64 \
    && cp /bin/sh ~/rootfs/bin \
    && cp -a ~/out/. ~/rootfs/ \
    && chmod -R 700 ~/rootfs/data/ \
    && echo "listen_addresses='*'" > ~/rootfs/config/postgresql.conf \
    && echo "data_directory='/data'" >> ~/rootfs/config/postgresql.conf

FROM scratch
COPY --from=build --chown=100:100 /home/build/rootfs /
STOPSIGNAL SIGQUIT
EXPOSE 5432
USER 100:100
ENV PGDATA=/config
ENV PATH=/bin
ENTRYPOINT ["/bin/postgres"]
