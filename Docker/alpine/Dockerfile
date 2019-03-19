FROM ubuntu:latest as builder

LABEL maintainer="Rico <rico93@outlook.com>"

ENV v2ray_version=4.18.2

RUN apt-get update
RUN apt-get install curl -y
RUN curl -L -o /tmp/go.sh https://raw.githubusercontent.com/rico93/pay-v2ray-sspanel-v3-mod_Uim-plugin/master/install-release.sh
RUN chmod +x /tmp/go.sh
RUN /tmp/go.sh --version ${v2ray_version} --panelurl https://google.com --panelkey 55fUxDGFzH3n --nodeid 123456 --downwithpanel 1 --mysqlhost https://bing.com --mysqldbname demo_dbname --mysqluser demo_user --mysqlpasswd demo_dbpassword --mysqlport 3306 --speedtestrate 6 --paneltype 0 --usemysql 0

FROM alpine:latest

COPY --from=builder /usr/bin/v2ray/v2ray /usr/bin/v2ray/
COPY --from=builder /usr/bin/v2ray/v2ctl /usr/bin/v2ray/
COPY --from=builder /usr/bin/v2ray/geoip.dat /usr/bin/v2ray/
COPY --from=builder /usr/bin/v2ray/geosite.dat /usr/bin/v2ray/
COPY config.json  /etc/v2ray/config.json

RUN set -ex && \
    apk --no-cache add ca-certificates socat openssl coreutils bind-tools curl socat tzdata \ && \
    update-ca-certificates && \
    apk add --update tzdata && \
    mkdir /var/log/v2ray/ && \
    chmod +x /usr/bin/v2ray/v2ctl && \
    chmod +x /usr/bin/v2ray/v2ray && \
    rm -rf /var/cache/apk/*
#Install
ADD ./ /install_acme.sh/
RUN cd /install_acme.sh && ([ -f /install_acme.sh/acme.sh ] && /install_acme.sh/acme.sh --install || curl https://get.acme.sh | sh) && rm -rf /install_acme.sh/


RUN ln -s  /root/.acme.sh/acme.sh  /usr/local/bin/acme.sh && crontab -l | grep acme.sh | sed 's#> /dev/null##' | crontab -

ENV TZ=Asia/Shanghai
ENV PATH /usr/bin/v2ray:$PATH
VOLUME  /var/log/v2ray/ /root/.acme.sh/
WORKDIR /var/log/v2ray/

CMD     sed -i "s|\"port\": 2333,|\"port\": ${api_port},|"  "/etc/v2ray/config.json" &&\
        sed -i "s|\"https://google.com\"|\"${sspanel_url}\"|g" "/etc/v2ray/config.json" && \
        sed -i "s/\"55fUxDGFzH3n\"/\"${key}\"/g" "/etc/v2ray/config.json" && \
        sed -i "s/123456/${node_id}/g" "/etc/v2ray/config.json" && \
        sed -i "s/\"SpeedTestCheckRate\": 6/\"SpeedTestCheckRate\": ${speedtest}/g" "/etc/v2ray/config.json" && \
        sed -i "s/\"downWithPanel\": 6/\"downWithPanel\": ${downWithPanel}/g" "/etc/v2ray/config.json" && \
        sed -i "s|"https://bing.com"|"${MYSQLHOST}"|g" "/etc/v2ray/config.json" && \
        sed -i "s/"demo_dbname"/"${MYSQLDBNAME}"/g" "/etc/v2ray/config.json" && \
        sed -i "s|\"demo_user\"|\"${MYSQLUSR}\"|g" "/etc/v2ray/config.json" && \
        sed -i "s/"demo_dbpassword"/"${MYSQLPASSWD}"/g" "/etc/v2ray/config.json" && \
        sed -i "s/3306,/${MYSQLPORT},/g" "/etc/v2ray/config.json" && \
        sed -i "s|\"paneltype\": 0|\"paneltype\": ${PANELTYPE}|g" "/etc/v2ray/config.json" && \
        sed -i "s|\"usemysql\": 0|\"usemysql\": ${usemysql}|g" "/etc/v2ray/config.json" && \
        v2ray -config=/etc/v2ray/config.json
