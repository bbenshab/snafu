FROM registry.access.redhat.com/ubi8:latest as builder
COPY image_resources/centos8.repo /etc/yum.repos.d/centos8.repo
RUN dnf install -y --enablerepo=centos8 make gcc libaio zlib-devel libaio-devel
RUN curl -L https://github.com/axboe/fio/archive/fio-3.19.tar.gz | tar xzf -
RUN pushd fio-fio-3.19 && make -j2

FROM registry.access.redhat.com/ubi8:latest
COPY --from=builder /fio-fio-3.19/fio /usr/local/bin/fio
RUN dnf install -y --nodocs git python3-pip python3-requests python3-numpy libaio zlib procps-ng iproute net-tools ethtool nmap iputils && dnf clean all
RUN ln -s /usr/bin/python3 /usr/bin/python
COPY . /opt/snafu/
RUN pip3 install --upgrade-strategy=only-if-needed -r /opt/snafu/requirements.txt
