
FROM tomcat:9.0-jdk8-openjdk-bullseye

LABEL maintainer="jeremy.cook@cloudacademy.com"

COPY splunkforwarder-7.2.1-be11b2c46e23-linux-2.6-amd64.deb /tmp/

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        procps \
        vim \
        supervisor \
    && rm -rf /var/lib/apt/lists/*

RUN dpkg -i /tmp/splunkforwarder-7.2.1-be11b2c46e23-linux-2.6-amd64.deb \
    || (apt-get update \
        && apt-get install -f -y \
        && dpkg -i /tmp/splunkforwarder-7.2.1-be11b2c46e23-linux-2.6-amd64.deb)

RUN /opt/splunkforwarder/bin/splunk start \
    --accept-license \
    --answer-yes \
    --auto-ports \
    --no-prompt

COPY inputs.conf /opt/splunkforwarder/etc/system/local/
COPY outputs.conf /opt/splunkforwarder/etc/system/local/
COPY user-seed.conf /opt/splunkforwarder/etc/system/local/

RUN mkdir -p /var/log/webapp

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

COPY *.war /usr/local/tomcat/webapps/

# CMD ["catalina.sh", "run"]
ENTRYPOINT ["/usr/bin/supervisord"]
