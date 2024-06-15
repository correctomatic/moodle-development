# Use the official Alpine image as a base
FROM alpine:latest

RUN apk --no-cache add dnsmasq

# dnsmasq configuration
RUN mkdir -p /etc/dnsmasq.d
COPY dnsmasq.conf /etc/dnsmasq.d/dnsmasq.conf

EXPOSE 53/udp

# Debug
RUN apk add net-tools vim

CMD ["/usr/sbin/dnsmasq","-k","--conf-file=/etc/dnsmasq.d/dnsmasq.conf"]
