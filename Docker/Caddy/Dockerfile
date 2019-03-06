LABEL maintainer="Rico <rico93@outlook.com>"

ENV v2ray_version=4.14.0

RUN apt-get update
RUN apt-get install curl -y
RUN curl -L -o /tmp/go.sh https://raw.githubusercontent.com/rico93/v2ray-core/4.12.0_ips/release/install-release.sh
RUN chmod +x /tmp/go.sh
RUN /tmp/go.sh --version ${v2ray_version} --panelurl https://google.com --panelkey 55fUxDGFzH3n --nodeid 123456

FROM jessestuart/caddy-cloudflare:v0.11.0

COPY --from=builder /usr/bin/v2ray/v2ray /usr/bin/v2ray/
COPY --from=builder /usr/bin/v2ray/v2ctl /usr/bin/v2ray/
COPY --from=builder /usr/bin/v2ray/geoip.dat /usr/bin/v2ray/
COPY --from=builder /usr/bin/v2ray/geosite.dat /usr/bin/v2ray/
COPY --from=builder /etc/v2ray/config.json  /etc/v2ray/config.json

RUN set -ex && \
    apk update && \
    apk upgrade && \
    apk --no-cache add ca-certificates && \
    update-ca-certificates && \
    apk --no-cache add --update tzdata && \
    mkdir /var/log/v2ray/ &&\
    chmod +x /usr/bin/v2ray/v2ctl && \
    chmod +x /usr/bin/v2ray/v2ray && \
    mkdir /srv/www

ENV TZ=Asia/Shanghai
ENV PATH /usr/bin/v2ray:$PATH

COPY index.html /srv/www/index.html
EXPOSE 80 443 2015
VOLUME /root/.caddy /srv /var/log/v2ray
WORKDIR /srv

CMD     sed -i "s|\"port\": 2333,|\"port\": ${api_port},|"  "/etc/v2ray/config.json" &&\
        sed -i "s|"https://google.com"|"${sspanel_url}"|g" "/etc/v2ray/config.json" && \
        sed -i "s/"55fUxDGFzH3n"/"${key}"/g" "/etc/v2ray/config.json" && \
        sed -i "s/123456/${node_id}/g" "/etc/v2ray/config.json" && \
        /bin/parant
        (nohup v2ray -config=/etc/v2ray/config.json >/dev/null 2>&1 & )
ENTRYPOINT ["/bin/parent", "caddy"]
CMD ["--conf", "/etc/Caddyfile", "--log", "stdout", "--agree=$ACME_AGREE"]