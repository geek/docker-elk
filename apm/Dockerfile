FROM centos:7

ENV APM_HOME /usr/share/apm-server
ENV APM_VERSION 6.1.1
ENV ELASTIC_CONTAINER false
ENV PATH ${APM_HOME}:$PATH

RUN yum update -y && yum install -y curl && yum clean all
RUN curl -Lso - https://artifacts.elastic.co/downloads/apm-server/apm-server-${APM_VERSION}-linux-x86_64.tar.gz | \
      tar zxf - -C /tmp \
      && mv /tmp/apm-server-${APM_VERSION}-linux-x86_64 ${APM_HOME}

ADD entrypoint /usr/local/bin

WORKDIR ${APM_HOME}
RUN mkdir data logs && \
    find ${APM_HOME} -type d -exec chmod 0750 {} \; && \
    find ${APM_HOME} -type f -exec chmod 0640 {} \; && \
    chmod 0750 /usr/local/bin/entrypoint && \
    chmod 0750 /usr/share/apm-server/apm-server && \
    chmod 0770 data logs

EXPOSE 8200

ENTRYPOINT ["/usr/local/bin/entrypoint"]
CMD ["-e"]
