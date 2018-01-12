FROM centos:7

ENV APM_HOME /usr/share/apm-server
ENV APM_VERSION 6.1.1
ENV ELASTIC_CONTAINER true
ENV PATH ${APM_HOME}:$PATH

RUN yum update -y && yum install -y curl && yum clean all
RUN curl -Lso - https://artifacts.elastic.co/downloads/apm-server/apm-server-${APM_VERSION}-linux-x86_64.tar.gz | \
      tar zxf - -C /tmp \
      && mv /tmp/apm-server-${APM_VERSION}-linux-x86_64 ${APM_HOME}

# Add entrypoint wrapper script
ADD entrypoint /usr/local/bin

# Provide a non-root user.
RUN groupadd --gid 1000 apm-server && \
    useradd -M --uid 1000 --gid 1000 --home ${APM_HOME} apm-server

WORKDIR ${APM_HOME}
RUN mkdir data logs && \
    chown -R root:apm-server . && \
    find ${APM_HOME} -type d -exec chmod 0750 {} \; && \
    find ${APM_HOME} -type f -exec chmod 0640 {} \; && \
    chmod 0770 data logs

USER apm-server

EXPOSE 8200

ENTRYPOINT ["/usr/local/bin/entrypoint"]
CMD ["-e"]
