FROM debian:bullseye-slim

# Cài đặt các công cụ cần thiết và thêm kho PostgreSQL chính thức
RUN apt-get update && apt-get install -y \
    wget gnupg2 lsb-release curl \
    && echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt-get update && apt-get install -y \
    python3 python3-pip postgresql-16 postgresql-client-16 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Cài đặt Patroni
RUN pip3 install patroni[etcd] protobuf==3.20.3 etcd3 psycopg2-binary psycopg==3.0.0

# Tạo thư mục dữ liệu PostgreSQL
RUN mkdir -p /var/lib/postgresql/data && chown postgres:postgres /var/lib/postgresql/data

RUN useradd -ms /bin/bash patroni
RUN chown patroni:patroni /var/lib/postgresql/data

USER patroni



# Chạy Patroni
CMD ["patroni", "/config/patroni.yml"]